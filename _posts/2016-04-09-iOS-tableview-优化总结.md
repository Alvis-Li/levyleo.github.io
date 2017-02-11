---
layout: post
title: iOS tableview 优化总结
---

```
根据网络上的优化方法进行了总括。并未仔细进行语言组织。正在这些优化方法进行学习，见另一篇文章 提高app流畅度

1、cell子控件创建写在 initWithStyle:reuseIdentifier

2、后台计算高度,布局。放在集合中下次使用。（计算高度是件很麻烦的事，分散计算，减少计算次数）

3、有一些显示的内容有富文本，特别是从HTML 转化为属性字符串时候。

解决方案，后台提前转化需要的属性字符串，然后缓存起来避免重复转化带来的CPU性能消耗。可以参考DTCoreText从HTML转化属性字符串的思路，他就是GCD后台转化的。

4、图片圆角，阴影等操作，会引起离屏渲染。对CPU性能消耗。

解决方案：用一个图片盖上，或者后台就把图片绘制成圆角图片显示。

5、一些显示很多图片的地方，服务器的图片很大，不是你控件的大小。

解决方案：服务器返回控件宽高的图片，比如七牛可以在图片路径拼接参数来获取指定宽高。

6、视图层次复杂情况下，CPU把它们混合会很消耗资源

解决方案：如果不能避免，可以把你的视图绘制成一张图片来显示，当然渲染的过程在后台。可以参考VVeboTableViewDemo的思路。

7、不用SB 和Xib 来创建cell 用纯代码来创建

8、抛弃自动布局用坐标来计算布局

9、把一些影响CPU的操作放到后台来操作，然后缓存一切可以缓存的东西

10、cell的高度没有缓存，这肯定有影响

11、短时间刷新2次，肯定会卡 。 防止短时间内多次刷新

12、反复创建View是不可取的…一般都会在初始化的时候全部创建，然后控制显示和隐藏

13、圆角。。小心别让圆角成了你列表的帧数杀手

14、修改下下载图片的尺寸。

15、换成子线程写入文件

16、滚动时网络请求的位置

17、把赋值和计算布局分离。这样让tableView:cellForRowAtIndexPath:方法只负责赋值，tableView:heightForRowAtIndexPath:方法只负责计算高度。两者都需要尽可能的简单易算。

18、获得数据后，直接先根据数据源计算出对应的布局，并缓存到数据源中，这样在tableView:heightForRowAtIndexPath:方法中就直接返回高度，而不需要每次都计算了。

19、给自定义的Cell添加draw方法，（当然也可以重写drawRect）然后在方法体中实现，各个信息都是根据之前算好的布局进行绘制的。这里是需要异步绘制，但如果在重写drawRect方法就不需要用GCD异步线程了，因为drawRect本来就是异步绘制的。对于图文混排的绘制，可以移步Google，研究下CoreText。

20、滑动UITableView时，按需加载对应的内容。//按需加载 - 如果目标行与当前行相差超过指定行数，只在目标滚动范围的前后指定3行加载。

- (void)scrollViewWillEndDragging:(UIScrollView *)scrollView withVelocity:(CGPoint)velocity targetContentOffset:(inout CGPoint *)targetContentOffset{

    NSIndexPath *ip = [self indexPathForRowAtPoint:CGPointMake(0, targetContentOffset->y)];

    NSIndexPath *cip = [[self indexPathsForVisibleRows] firstObject];

    NSInteger skipCount = 8;

    if (labs(cip.row-ip.row)>skipCount) {

        NSArray *temp = [self indexPathsForRowsInRect:CGRectMake(0, targetContentOffset->y, self.width, self.height)];

        NSMutableArray *arr = [NSMutableArray arrayWithArray:temp];

        if (velocity.y<0) {

            NSIndexPath *indexPath = [temp lastObject];

            if (indexPath.row+3<datas.count) {

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row+1 inSection:0]];

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row+2 inSection:0]];

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row+3 inSection:0]];

            }

        } else {

            NSIndexPath *indexPath = [temp firstObject];

            if (indexPath.row>3) {

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row-3 inSection:0]];

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row-2 inSection:0]];

                [arr addObject:[NSIndexPath indexPathForRow:indexPath.row-1 inSection:0]];

            }

        }

        [needLoadArr addObjectsFromArray:arr];

    }

}

记得在tableView:cellForRowAtIndexPath:方法中加入判断：

if (needLoadArr.count>0&&[needLoadArr indexOfObject:indexPath]==NSNotFound) {

    [cell clear];

    return;

}

滚动很快时，只加载目标范围内的Cell，这样按需加载，极大的提高流畅度。

参考：https://github.com/johnil/VVeboTableViewDemo

21、提前计算并缓存好高度（布局），因为heightForRowAtIndexPath:是调用最频繁的方法；

22、异步绘制，遇到复杂界面，遇到性能瓶颈时，可能就是突破口；

23、滑动时按需加载，这个在大量图片展示，网络加载的时候很管用！（SDWebImage已经实现异步加载，配合这条性能杠杠的）。

24、 正确使用reuseIdentifier来重用Cells

25、尽量使所有的view opaque，包括Cell自身

26、尽量少用或不用透明图层

27、如果Cell内现实的内容来自web，使用异步加载，缓存请求结果

28、减少subviews的数量

29、在heightForRowAtIndexPath:中尽量不使用cellForRowAtIndexPath:，如果你需要用到它，只用一次然后缓存结果

30、尽量少用addView给Cell动态添加View，可以初始化时就添加，然后通过hide来控制是否显示

31、要想提高效率，还是手动写有用！抛开Xib、Storyboard需要系统自动转码，给系统多加了一层负担不谈，自定义Cell的绘制更是无从下手

32、只定义一种Cell的好处

减少代码量，减少Nib文件的数量，统一一个Nib文件定义Cell，容易修改、维护。

基于Cell的重用，真正运行时铺满屏幕所需的Cell数量大致是固定的，设为N个。所以如果如果只有一种Cell，那就是只有N个Cell的实例；但是如果有M种Cell，那么运行时最多可能会是“M x N = MN”个Cell的实例，虽然可能并不会占用太多内存，但是能少点不是更好吗。

33、善用hidden隐藏（显示）Subview。

34、提前计算并缓存每个Cell的高度

35、在Model（Entity）中计算并保存Cell的高度、其实，在Model（Entity）中保存UI的参数是很奇怪的=。=（最好放在ViewModel中，就是MVVM模式的

36、提前创建真正显示的、需要加工的数据并缓存、Cell中显示的内容，很多时候可能并不是直接从服务器拿到的数据，而是经过“加工”的数据。如本文中的“动态”也，每个Cell的标题、正文都有可点击的连接Link、表情图片等富文本内容，而我们一般用NSAttributeString类来显示。

既然每次都会用到，倒不如在获取到数据的时候就创建、加工好这些内容，等到需要现实的时候，直接拿来用不就行了。

37、缓存View! 是的，当Cell中的部分View是非常独立的，并且不便于重用的，而且“体积”非常小，在内存可控的前提下，我们完全可以将这些view缓存起来！方法当然也是将缓存的view放在Entity中~。

38、尽量设置Cell的view为opaque，避免GPU对Cell下面的内容也进行绘制。

39、避免大量的图片缩放、颜色渐变等。

40、避免同步的从网络、文件获取数据（这个是必须的=。=）

41、用shadowPath创建阴影。

42、尽量减少subview的数量，如多用drawRect绘制元素，替代用view显示。

43、尽量显示“大小刚好合适”的图片资源。

44、缓存一切可以缓存的！就是“用空间替换时间”！

45、在UITableView的Delegate、DataSource方法中，减少任何不必要的操作

46、针对NSDateFormatter时间开销出了重用对象外，尽量避免采用其处理多个日期格式.当然针对日期格式处理如果需要提高更多速度，可以直接采用C,可以采用第三方库来规避这个问题..

47、大量使用imageNamed方式会在不需要缓存的地方额外增加开销CPU的时间来做这件事.当应用程序需要加载一张比较大的图片并且使用一次性，那么其实是没有必要去缓存这个图片的，用imageWithContentsOfFile是最为经济的方式,这样不会因为UIImage元素较多情况下，CPU会被逐个分散在不必要缓存上浪费过多时间.

48、针对单个view 尽量不要在viewWillAppear费时的操作，viewWillAppear在 view 显示之前被调用，出于效率考虑，在这个方法中不要处理复杂费时的事情；只应该在这个方法设置 view 的显示属性之类的简单事情，比如背景色，字体等。不然，用户会明显感觉到 view 显示迟钝.

49、reloadData 方法尽量不要调用

50、如果 row 的高度都一定，那就删除代理中的这个 tableView:heightForRowAtIndexPath: 方法，设置 Table View 的 rowHeight 属性，相似的 numberOfRowsInSection: 系列的方法，我就不都写出来了。苹果的文档里介绍这样也可以减少了调用时间。

51、用 “空间换时间” 、将计算行高的时间提前到从服务器搂回数据的时候，计算完了高度一并写回数据库。

52、直接设置 image view 的 contentMode 属性要 image view 自己 压缩。这是一个很取巧的方法，但是对 table view 的滚动速度也会造成 不容忽视 的影响。对图片变形需要对图片做 transform ，每次压缩图片都要对图片乘以一个变换矩阵，如果你的图片很多，这个计算量是不同忽视的。

从网络搂回来图片后先根据需要显示的图片大小切成合适大小的图，每次只显示处理过大小的图片，当查看大图时在显示大图。如果服务器能直接返回预处理好的小图和图片的大小更好。

53、在设置 view 的 frame 时，在高分屏避免出现 21.3，6.7这样的小数，尤其是 x，y坐标，用 ceil 或 floor 或 round 取整。每 0.5 个点对应一个 pixel，0.3,0.7这样的就难为 iPhone 了，低分屏不要出现小数。

54、手动绘制方法，不是直接子类化 UITableViewCell，然后覆盖 drawRect: 方法，这样你会得到一个大黑块！因为 cell 中不是只有一个 content view。如果不了解 cell 的层次结构，可以用 Reveal 去看下。GPU 不喜欢 透明，所以所有的绘图一定要弄成不透明，对于圆角和阴影这些的可以截个伪透明的小图然后绘制上去。在layer的回调里一定也只做绘图，不做计算！

55、不要用AutoLayout，不要用AutoLayout，不要用AutoLayout。我们进行手动布局，都是非常简单的线性计算，CPU就不用浪费那么时间，CPU的压力不会很大，从而平衡了CPU的负载。

56、当一个控件本身是不透明的，注意设定opaque = YES，这样子可以避免无用的alpha通道合成，降低GPU负载。

57、对控件设置cornerRadius后对其进行clip或mask操作时，会导致offscreen rendering，而这个是在GPU中进行的，所以快速滑动tableView时，假如圆角对象较多，会导致GPU负载大增。这时候我们可以设置layer的shouldRasterize属性为YES，可以将负载转移给CPU。更为彻底的做法是直接在后台绘制圆角图片然后输出到主线程显示，避免使用圆角、阴影、遮罩等属性

58、将GPU的部分渲染转接给CPU，那么如何转接呢？我们可以在单个控件中重载drawRect:方法,直接将文字和图片绘制然后输出到主线程上。也可以设置Dictionary给Attributes赋值达到自己想要的文字效果。这段代码禁用了一些混合操作，减轻了GPU的负担，从而使UITableView滑动更加流畅。

59、尽量用轻量的对象代替重量的对象，可以对性能有所优化。比如 CALayer 比 UIView 要轻量许多，那么不需要响应触摸事件的控件，用 CALayer 显示会更加合适。如果对象不涉及 UI 操作，则尽量放到后台线程去创建，但可惜的是包含有 CALayer 的控件，都只能在主线程创建和操作。

60、尽量推迟对象创建的时间，并把对象的创建分散到多个任务中去。尽管这实现起来比较麻烦，并且带来的优势并不多，但如果有能力做，还是要尽量尝试一下。如果对象可以复用，并且复用的代价比释放、创建新对象要小，那么这类对象应当尽量放到一个缓存池里复用。

61、在应用中，应该尽量减少不必要的属性修改。

62、当视图层次调整时，UIView、CALayer 之间会出现很多方法调用与通知，所以在优化性能时，应该尽量避免调整视图层次、添加和移除视图。

63、对象的销毁虽然消耗资源不多，但累积起来也是不容忽视的。通常当容器类持有大量对象时，其销毁时的资源消耗就非常明显。同样的，如果对象可以放到后台线程去释放，那就挪到后台线程去。这里有个小 Tip：把对象捕获到 block 中，然后扔到后台队列去随便发送个消息以避免编译器警告，就可以让对象在后台线程销毁了。

NSArray *tmp = self.array;

self.array = nil;

dispatch_async(queue, ^{

    [tmp class];

});

64、尽量提前计算好布局，在需要时一次性调整好对应属性，而不要多次、频繁的计算和调整这些属性。

65、如果你对文本显示没有特殊要求，可以参考下 UILabel 内部的实现方式：用 [NSAttributedString boundingRectWithSize:options:context:] 来计算文本宽高，用 -[NSAttributedString drawWithRect:options:context:] 来绘制文本。尽管这两个方法性能不错，但仍旧需要放到后台线程进行以避免阻塞主线程。

66、屏幕上能看到的所有文本内容控件，包括 UIWebView，在底层都是通过 CoreText 排版、绘制为 Bitmap 显示的。常见的文本控件 （UILabel、UITextView 等），其排版和绘制都是在主线程进行的，当显示大量文本时，CPU 的压力会非常大。对此解决方案只有一个，那就是自定义文本控件，用 TextKit 或最底层的 CoreText 对文本异步绘制。尽管这实现起来非常麻烦，但其带来的优势也非常大，CoreText 对象创建好后，能直接获取文本的宽高等信息，避免了多次计算（调整 UILabel 大小时算一遍、UILabel 绘制时内部再算一遍）；CoreText 对象占用内存较少，可以缓存下来以备稍后多次渲染

67、当你用 UIImage 或 CGImageSource 的那几个方法创建图片时，图片数据并不会立刻解码。图片设置到 UIImageView 或者 CALayer.contents 中去，并且 CALayer 被提交到 GPU 前，CGImage 中的数据才会得到解码。这一步是发生在主线程的，并且不可避免。如果想要绕开这个机制，常见的做法是在后台线程先把图片绘制到 CGBitmapContext 中，然后从 Bitmap 直接创建图片。目前常见的网络图片库都自带这个功能。

68、图像的绘制通常是指用那些以 CG 开头的方法把图像绘制到画布中，然后从画布创建图片并显示这样一个过程。这个最常见的地方就是 [UIView drawRect:] 里面了。由于 CoreGraphic 方法通常都是线程安全的，所以图像的绘制可以很容易的放到后台线程进行。一个简单异步绘制的过程大致如下（实际情况会比这个复杂得多，但原理基本一致）：

- (void)display {

    dispatch_async(backgroundQueue, ^{

        CGContextRef ctx = CGBitmapContextCreate(...);

        // draw in context...

        CGImageRef img = CGBitmapContextCreateImage(ctx);

        CFRelease(ctx);

        dispatch_async(mainQueue, ^{

            layer.contents = img;

        });

    });

}

69、尽量减少在短时间内大量图片的显示，尽可能将多张图片合成为一张进行显示。当图片过大，超过 GPU 的最大纹理尺寸时，图片需要先由 CPU 进行预处理，这对 CPU 和 GPU 都会带来额外的资源消耗。目前来说，iPhone 4S 以上机型，纹理尺寸上限都是 4096x4096，更详细的资料可以看这里：iosres.com。所以，尽量不要让图片和视图的大小超过这个值。

70、应用应当尽量减少视图数量和层次，并在不透明的视图里标明 opaque 属性以避免无用的 Alpha 通道合成。当然，这也可以用上面的方法，把多个视图预先渲染为一张图片来显示。

71、CALayer 的 border、圆角、阴影、遮罩（mask），CASharpLayer 的矢量图形显示，通常会触发离屏渲染（offscreen rendering），而离屏渲染通常发生在 GPU 中。当一个列表视图中出现大量圆角的 CALayer，并且快速滑动时，可以观察到 GPU 资源已经占满，而 CPU 资源消耗很少。这时界面仍然能正常滑动，但平均帧数会降到很低。为了避免这种情况，可以尝试开启 CALayer.shouldRasterize 属性，但这会把原本离屏渲染的操作转嫁到 CPU 上去。对于只需要圆角的某些场合，也可以用一张已经绘制好的圆角图片覆盖到原本视图上面来模拟相同的视觉效果。最彻底的解决办法，就是把需要显示的图形在后台线程绘制为图片，避免使用圆角、阴影、遮罩等属性。

72、对于通常的 TableView 来说，提前在后台计算好布局结果是非常重要的一个性能优化点。为了达到最高性能，你可能需要牺牲一些开发速度，不要用 Autolayout 等技术，少用 UILabel 等文本控件。

73、为了避免离屏渲染，你应当尽量避免使用 layer 的 border、corner、shadow、mask 等技术，而尽量在后台线程预先绘制好对应内容。当头像下载下来后，我会在后台线程将头像预先渲染为圆形并单独保存到一个 ImageCache 中去。

74、“过早的优化是万恶之源”，在需求未定，性能问题不明显时，没必要尝试做优化，而要尽量正确的实现功能。做性能优化时，也最好是走修改代码 -> Profile -> 修改代码这样一个流程，优先解决最值得优化的地方。如果你需要一个明确的 FPS 指示器，可以尝试一下 KMCGeigerCounter。对于 CPU 的卡顿，它可以通过内置的 CADisplayLink 检测出来；对于 GPU 带来的卡顿，它用了一个 1x1 的 SKView 来进行监视。这个项目有两个小问题：SKView 虽然能监视到 GPU 的卡顿，但引入 SKView 本身就会对 CPU/GPU 带来额外的一点的资源消耗；这个项目在 iOS 9 下有一些兼容问题，需要稍作调整。

我自己也写了个简单的 FPS 指示器：FPSLabel 只有几十行代码，仅用到了 CADisplayLink 来监视 CPU 的卡顿问题。虽然不如上面这个工具完善，但日常使用没有太大问题。

75、不透明的视图可以极大地提高渲染的速度。因此如非必要，可以将table cell及其子视图的opaque属性设为YES（默认值）。

其中的特例包括背景色，它的alpha值应该为1（例如不要使用clearColor）；图像的alpha值也应该为1，或者在画图时设为不透明。

76、不要重复创建不必要的table cell。

前面说了，UITableView只需要一屏幕的UITableViewCell对象即可。因此在cell不可见时，可以将其缓存起来，而在需要时继续使用它即可。cell被重用时，它内部绘制的内容并不会被自动清除，因此你可能需要调用setNeedsDisplayInRect:或setNeedsDisplay方法。

77、减少视图的数目、使用自定义的view，而非预定义的view，明显会快些。当然，最佳的解决办法还是继承UITableViewCell，并在其drawRect:中自行绘制：

- (void)drawRect:(CGRect)rect {

    if (image) {

        [image drawAtPoint:imagePoint];

        self.image =nil;

    } else {

        [placeHolder drawAtPoint:imagePoint];

    }

 

    [text drawInRect:textRect withFont:font lineBreakMode:UILineBreakModeTailTruncation];

}

  不过这样一来，你会发现选中一行后，这个cell就变蓝了，其中的内容就被挡住了。最简单的方法就是将cell的selectionStyle属性设为UITableViewCellSelectionStyleNone，这样就不会被高亮了。此外还可以创建CALayer，将内容绘制到layer上，然后对cell的contentView.layer调用addSublayer:方法。这个例子中，layer并不会显著影响性能，但如果layer透明，或者有圆角、变形等效果，就会影响到绘制速度了。解决办法可参见后面的预渲染图像。

78、不要做多余的绘制工作。

在实现drawRect:的时候，它的rect参数就是需要绘制的区域，这个区域之外的不需要进行绘制。例如上例中，就可以用CGRectIntersectsRect、CGRectIntersection或CGRectContainsRect判断是否需要绘制image和text，然后再调用绘制方法。

79、insertRowsAtIndexPaths:withRowAnimation:方法，插入新行需要在主线程执行，而一次插入很多行的话（例如50行），会长时间阻塞主线程。而换成reloadData方法的话，瞬间就处理完了。

80、尽量不调用‘cellForRowAtIndexPath’。调用cellForRowAtIndexPath会导致cell缓存失效

81、cache尽可能多的东西，包括行高

82、缓存不大可能改变但是需要经常读取的东西。远端服务器的响应、图片、计算结果

83、重用大开销对象。对于初始化很慢的对象通过添加属性的方式保持该对象，保证只被初始化一次，多次复用。如NSDataFormatter。

84、方法指针缓存。如果一个方法在一个循环次数非常多的循环中使用，在进入循环前使用methodForSelector获取该方法的IMP，在循环体中直接调用该IMP。

85、对图片数据进行decode。在子线程中设置image的大小后，在imageview中使用缩放后的image。原因：由于UIImage的imageWithData函数是每次画图的时候才将Data解压成ARGB的图像，所以在每次画图的时候，会有一个解压操作，UIImage初始化后仅仅是把图片加载到内存中，而实际的解码和重采样是在图片需要显示时才进行：

//图片重采样，在子线程中进行

CGSize itemSize = CGSizeMake(width, height);//实际要缩放的大小

UIGraphicsBeginImageContext(itemSize);

CGRect imageRect = CGRectMake(0.0, 0.0, itemSize.width, itemSize.height);

[image drawInRect:imageRect];

UIImage newImage = UIGraphicsGetImageFromCurrentImageContext(); //重采样后的图片

UIGraphicsEndImageContext();

86、使用富文本标签代价是很昂贵的

费尽周折用富文本标签，代价太昂贵了。尽可能地避免使用这个。问问你自己是否真的需要这个。如果是的话，尽可能的做缓存。

87、不要过多使用Xib（如果可以的话使用storyboard）

如果要使用xib就要小心一点。当你加载一个Xib，整个的内容会被加载到内存中（图片，隐藏的views）。但是这在storyboard中不会发生他只会实例化当前要用的东西。

88、使用CoreGraphics并在一个view的drawRect的方法中写你的UI代码。

89、可以识别tableview禁止或者减速滑动结束的时候进行异步加载图片

- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate

- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView

以下方法来执行异步加载操作

      //获取可见部分的对象

       NSArray *visiblePaths = [self.tableView indexPathsForVisibleRows];

        for (NSIndexPath *indexPath in visiblePaths)

        {

           //获取的dataSource里面的对象，并且判断加载完成的不需要再次异步加载

             <code>

        }

 

同时在cell绘制中也做限制

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath

 

         if (self.tableView.dragging == NO && self.tableView.decelerating == NO)

            {

               //开始异步加载图片

                <code>

            }

 

如果tableview 停止滑动的时候开始异步加载图片

 

90、在内存紧张的情况下释放调所有的异步线程，以保证的你的app不会被系统强制关闭

- (void)didReceiveMemoryWarning{

//  释放调异步加载图片的线程以及所有图片资源对象

<code>

}

别忘记销毁的时候手动把所有的使用到的代理设置nil。

91、尽量少用绘制的代码（很多人想吐槽，我的cell样式很多不能用图片来完成的，骚年，你错了，微信App的UI元素很多都是用图片组合出来的）

92、尽量少改变UI控件的frame，如果对长文本的UITextView来说，或者你用高端的CoreText，当你改变frame，都会调用layoutSubViews，这个是无疑的，难倒你用CoreText不需要重绘嘛？（解决方案：我猜测可以用多样式UI控件去满足你多样化的业务展示）

93、少用reloadData方法，但是用insert reloadRow方法也会重新调用计算cell高度的方法，但是这里有点不一样，只是刷新可见cells的刷新，而不是整个tableView的cell，不过还是有点消耗资源。

94、容易忽视的stringWithFormat:stringWithFormat: 操作也会造成不可忽视的开销。我们可以用 C Style 的方式来构造我们的字符串！事实证明这能减少不小开销，如果你的 CustomCell 方法中有大量的 stringWithFormat: 方法，那么性能提升的效果应该非常明显。下面这段代码可以用来进行 C 和 ObjC 字符串的转换：

char cString[255];    // 这里要根据实际情况开辟大小  

sprintf(string, "Click: %d", numberOfClick);  

NSString *objCString = [[NNString alloc] initWithUTF8String:cString];  

95、Gif 图片本来就比较吃资源，满屏的 Cell 每个都有一个 GIF 在动，想想就酸爽。一开始是使用 FlAnimatedImage 在每个 Cell 中添加一个 GIF View 。用几个动画就能做出一样的效果，果断换了解决方案，换完之后简直顺畅。

96、不要忘记scrollViewDidScroll。随便滚动一下， scrollViewDidScroll: 就调用了N次。果断转移代码，想了下丢到了 scrollViewWillEndDragging:withVelocity:targetContentOffset: 代理方法中去了，利用这个 targetContentOffset 就能解决啦。和 CellForRowAtIndexPath: 方法一样，这个方法调用的频率也很高，迫不得已不在在里面放耗时的代码。

97、图片加载还有个小技巧就是当图片在异步加载的时候，图片所在的 Cell 滚动出了屏幕，那么立马把这个下载的操作给 Cancel 掉，避免不必要的资源消耗。

98、不要拉伸 Cell 中的图片

99、如果 Cell 有阴影，尽量使用图片而不是操作 layer 来绘制。

100、添加一个变量如userDragging，在 willBeginDragging 中设为YES，didEndDragging 中设为NO。那么tableView: cellForRowAtIndexPath:方法中，是否 load 图片的逻辑就是：

if (!self.userDragging && tableView.decelerating) {  

    cell.imageView.image = nil;

} else {

    // code for loading image from network or disk

}
```

