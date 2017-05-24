---
layout: post
title: class-dump 获取别人app的头文件
---

安装class-dump：

http://stevenygard.com/projects/class-dump/

下载[class-dump-3.5.dmg](http://stevenygard.com/download/class-dump-3.5.dmg)

将class-dump复制到/usr/local/bin/目录下即可

获取头文件，先创建个文件夹用来保存取到的文件，然后切到此目录下

```
class-dump -H /Applications/WeChat.app/Contents/MacOS/WeChat
```

即可获取头文件

[参照](http://ios.jobbole.com/84765/)