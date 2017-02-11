---
layout: post
title: MIDI and audio programming on OS X and iOS
---

Network MIDI on iOS - Part 1

This is an app I wrote to try out some ideas for networked MIDI on iPhone and iPad. It connects to a host computer running OS X Tiger or later (see the documentation for Apple's Audio MIDI Setup application), or any compatible RTP MIDI host such as [rtpMIDI for Windows](http://www.tobias-erichsen.de/rtpMIDI.html).

![](/img/Network MIDI on iOS.png)

I will discuss the implementation details in further blog posts, as it introduces some useful concepts such as:

- Composing outgoing MIDI data in response to user input
- Processing incoming MIDI data in real time (with semaphores and lock free buffers)
- Finding network services with Bonjour

For now, here's the source code.

[XCode 4 Project (MediaFire download)](http://www.mediafire.com/file/wtcsts114owm42d/NetworkMIDI.zip)

Updated project for iOS 5.1 - 6 using ARC:

[XCode 4.51 Project (MediaFire download)](http://www.mediafire.com/?316g6ph767guez7)

Updated project for iOS 7 (should work back to iOS 5.1):

[Xcode 5.1 Project (MediaFire download)](http://www.mediafire.com/download/hc00ep92x5p2hia/NetworkMIDI_XCode_5.1.zip)

****

**Please note you need to run this on a device rather than the simulator.**

(Now seems to work OK on the iOS 7 simulator at least).



Network MIDI on iOS - Part 2

In this part, I will discuss finding and publishing network MIDI services using Bonjour, and creating the MIDI client using the CoreMIDI API. The source code for this project is available for download in [Part 1](http://antifluke.blogspot.com/2011/07/network-midi-on-ios-part-1.html) of this article.

The network MIDI services in OS X and iOS can be discovered in the same way as any other Bonjour service. In the initialiser of the MIDIController class (see MIDIController.m) an instance of [NSNetServiceBrowser](http://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSNetServiceBrowser_Class/Reference/Reference.html) is created, and instructed to search for services of the type MIDINetworkBonjourServiceType i.e. @"_apple-midi._udp". By implementing the delegate protocol for this browser, a MIDIController instance can monitor available services of this type on the network.

These services are exposed to the remainder of the application as a dictionary. In addition, various operations are provided to allow connection, disconnection etc. (see the section marked "Connection Management" in the implementation for details). The sample application uses these methods to provide a simple UI to connect to any detected services. The settings page is displayed by flipping the main view. From here, we can select one or more of the remote MIDI services available on the current LAN.

 ![](/img/Settings.png) ![](/img/Settings 2.png)

Note that connections can also be made from the other end. By opening Audio MIDI Setup on a Mac on the same LAN, we can browse to the iPhone and initiate a connection from the computer. In this instance, the MIDIController instance will inform the UI that a connection has been made via an NSNotification. If multiple connections are made to or from the same device, they are bridged in that:

- All incoming data from the network is merged as if it was coming from a single device
- Outgoing data from the device is sent to all network services

The settings view also allows the user to set the MIDI channel on which messages will be sent in response to actions from the main UI views. This is similarly the channel on which the app will listen for incoming MIDI commands.

The MIDIController initialiser also sets up the shared [MIDINetworkSession](http://developer.apple.com/library/ios/#documentation/CoreMidi/Reference/MIDINetworkSession_ClassReference/Reference/Reference.html) instance, which acts as a bridge between the network services and the CoreMIDI API. A MIDI client, input port and output port are then created. The input port is connected to the source endpoint of the MIDINetworkSession instance. Note that these endpoints are named from the perspective of the iOS application; the "source" endpoint is where data will be received from the network, and the "destination" endpoint is where the application will send data to the network.

To send data to the output port once a connection has been made, the app invokes the methods in the section marked Sending in the implementation file, namely:

-(void) allNotesOffOnChannel:(NSUInteger)channel;

-(void) sendChangeForController:(NSUInteger)controller onChannel:(NSUInteger)channel withValue:(NSUInteger)value;

-(void) sendNote:(NSUInteger)note on:(BOOL)on onChannel:(NSUInteger)channel withVelocity:(NSUInteger)velocity;

-(void) sendMMCCommand:(NSUInteger)command toDevice:(NSUInteger)device;

Internally, these use the writeMIDIPacket methods to construct a MIDIPacketList and send it via the output port created above to the MIDINetworkSession's destinationEndpoint. Care should be taken to construct the packet lists correctly, as sending garbage data (for instance where the packet count is set too high) will typically result in disconnection by the remote service.

The sendChangeForController:onChannel:withValue: method is invoked in response to user input from the controller tab:

![](/img/IMG_0207.PNG)

The sendNote:on:onChannel:withVelocity: method is invoked when the user presses the "keys" on the Notes tab:

![](/img/IMG_0208.PNG)

The sendMMCCommand:toDevice: method is invoked in response to button presses on the MMC tab:

![](/img/IMG_0209.PNG)

*Please note that the MMC command output hasn't been tested as I don't have any devices that respond to this part of the MIDI protocol. Let me know via the comments if you have any problems.*



Network MIDI on iOS - Part 3

In this part I will discuss how incoming MIDI is received into the application and subsequently processed. The source code for this project is available for download in [Part 1](http://antifluke.blogspot.com/2011/07/network-midi-on-ios-part-1.html) of this article.

The MIDI controller class (see MIDIController.h) provides a formal protocol, MIDIReceivedDelegate, and a corresponding delegate property. This defines the following methods:

\- (void) midiControllerUpdated:(Byte)controller onChannel:(Byte)channel toValue:(Byte)value;

\- (void) midiNoteOnOff:(Byte)note onChannel:(Byte)channel withVelocity:(Byte)velocity on:(BOOL)on;

These methods encapsulate the complexity of receiving MIDI data - but how is this accomplished behind the scenes?

When we created the MIDI client in part 2 of this article, we also created a MIDI input port. This was passed a pointer to a callback function, MIDIInputReadProc. This callback function is called by the operating system when MIDI data is received at the port. As a real time callback function which may be re-entrant when the system is under load, certain restrictions should be adhered to. In particular, this function should avoid:

- Allocating and deallocating memory
- Acquiring locks
- Performing lengthy operations

The function walks the list of MIDI packets received as follows:

For each MIDI packet received:

- The packet's length and data are written into a [structured circular buffer](http://en.wikipedia.org/wiki/Circular_buffer) (see MIDIPacketBuffer.h)
- A [Mach semaphore](http://developer.apple.com/library/mac/#documentation/Darwin/Conceptual/KernelProgramming/synchronization/synchronization.html) is incremented to signal the rest of the application that another packet is available for processing.

Note that the structured buffer in this example disregards the timestamp of the MIDI packet. This will be covered in a later example.

Another thread (see midiInputThreadProc) waits on this semaphore. If the semaphore is signalled, the length of the next MIDI packet is retrieved from the circular buffer, and then that amount of data is copied into a regular buffer.

This data is then parsed from this buffer. The following [MIDI commands](http://www.midi.org/techspecs/midimessages.php) are identified in this example:

- Control changes
- Note on/off

Other commands are discarded. When a recognised command is received, [Grand Central Dispatch](http://en.wikipedia.org/wiki/Grand_Central_Dispatch) is used to enqueue a block that will invoke the controller's delegate asynchronously on the main queue. The delegate will then respond to the invocation and perform any necessary UI updates etc.

By these means, the application decouples the processing of MIDI data from its receipt, and conforms with the requirements placed on it by the CoreMIDI port model.