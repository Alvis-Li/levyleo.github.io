---
layout: post
title: UIActivityViewController 自定义选项
---

UIActivityViewController 自定义选项 重写 UIActivity 类 

建议下载github上源码学习一下 

[https://github.com/samvermette/SVWebViewController](https://github.com/samvermette/SVWebViewController)

在这里主要是记录一下我使用 UIActivityViewController 遇到的问题, 原本按照demo上写的 ,我同时添加了三个选项, 朋友圈,好友,open in Safari, 刚开始时三个选项同时在上面一栏,查了好久才查到原来有个属性决定 它的位置 

\+ (UIActivityCategory)activityCategory

{

    return UIActivityCategoryShare;

}

 ![](http://images2015.cnblogs.com/blog/789894/201509/789894-20150901170306997-2067711126.png)