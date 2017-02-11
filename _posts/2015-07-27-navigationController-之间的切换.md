---
layout: post
title: navigationController 之间的切换
---

项目要实现从一个Navigation 下push出的第N层controller后 立即切换到另一个 Navigation下

例如：在微信的通讯录Nav中选择一个好友，进入好友的详细资料，点击发消息按钮后，进入聊天界面，这时你会发现点击左上返回按钮后pop到的时微信Nav,（另一个NavigationController）。

QQ也有这样的功能，但是没有微信做的好，仔细看就会发现 QQ 是通过 先pop到rootViewController后（不带动画的）然后tabbarController 切换Nav。在动画之前你会发现界面变化后push

 

我找到一种类似微信那种切换方法，大家可以看一下，有错误大家指正一下。

实现方法：先获取一个Nav的rootcontroller 即controllrt

然后：

        [self.navigationController setViewControllers:@[[self.navigationController.viewControllers firstObject]]];

        [controllrt.navigationController setViewControllers:@[controllrt,self]];

        [controllrt.tabBarController setSelectedViewController:controllrt.navigationController];