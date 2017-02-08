---
layout: post
title: 制作支持Carthage的Framework
---

参照:[Creating your first iOS Framework](https://robots.thoughtbot.com/creating-your-first-ios-framework#setting-up-the-xcode-project) 
注意点:
*1.选择Framework & Library时需要选择“Cocoa Touch Library”,查了一下carthage issues 好像静态库不支持(待测试)*
*2.Make sure to select the “Include Unit Tests” check box.必须要选中Unit Tests*
*3.需要设置scheme为shared状态.
 Click on the scheme on the top left of Xcode and select “Manage 
Schemes”. We need to mark our scheme as “shared” so that the project can
 be built with Carthage.*
*不设置会遇到Dependency "test4" has no shared framework schemes的错误*
![img](http://images2015.cnblogs.com/blog/789894/201611/789894-20161102110453111-1639788484.jpg)￼

![img](http://images2015.cnblogs.com/blog/789894/201611/789894-20161102110453111-26589201.jpg)￼

*4.将所做的修改提交到git,尤其是xcshareddata,不提交会遇到错误Scheme test is not currently configured for the clean action.*

> 因为使用的本地git库,所以项目中的Cartfile中使用
> `git "本地framework目录" "master"`
> 这里只是简单的创建framework,未做其他配置,要想真正的创建能实际用的framework,需要做其他一些配置,参考:[ios打包静态库，看这篇就够了](http://www.jianshu.com/p/13bf46df9387#)