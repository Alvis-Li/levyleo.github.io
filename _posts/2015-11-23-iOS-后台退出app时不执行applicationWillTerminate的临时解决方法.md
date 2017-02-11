---
layout: post
title: iOS 后台退出app时不执行applicationWillTerminate的临时解决方法
---

```
- (void)applicationDidEnterBackground:(UIApplication *)application {
    // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later.
    // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
    //实现一个可以后台运行几分钟的权限, 当用户在后台强制退出程序时就会走applicationWillTerminate 了.
    [[UIApplication sharedApplication] beginBackgroundTaskWithExpirationHandler:nil];
     
}
```

```
- (void)applicationWillTerminate:(UIApplication *)application {
    // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
    NSString * AddressBookIndex = @"AddressBook_index";
    NSString * AddressBookDataArray = @"AddressBook_dataArray";
    NSString * AddressBookPhonePerson = @"AddressBook_Person";
    [[NSUserDefaults standardUserDefaults] removeObjectForKey:AddressBookIndex];
    [[NSUserDefaults standardUserDefaults] removeObjectForKey:AddressBookDataArray];
    [[NSUserDefaults standardUserDefaults] removeObjectForKey:AddressBookPhonePerson];
     
}
```

