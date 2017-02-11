---
layout: post
title: 隐藏StatusBar 非setStatusBarHidden
---

```
 UIWindow * window = [[UIApplication sharedApplication].windows lastObject];

 

隐藏

[window setWindowLevel:UIWindowLevelStatusBar];

 

显示 

[window setWindowLevel:UIWindowLevelStatusBar];

 
```

