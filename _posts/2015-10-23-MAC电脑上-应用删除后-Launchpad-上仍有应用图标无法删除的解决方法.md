---
layout: post
title: MAC电脑上 应用删除后 Launchpad 上仍有应用图标无法删除的解决方法
---

应用删除后 Launchpad 上仍有应用图标上带有问号且无法删除时,可以将 launchpad 重置.

```
在终端输入:
defaults write com.apple.dock ResetLaunchPad -bool true 回车
killall Dock 回车
等待 LaunchPad 重启 .
```

