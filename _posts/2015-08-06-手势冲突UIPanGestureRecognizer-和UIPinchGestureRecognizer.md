---
layout: post
title: 手势冲突UIPanGestureRecognizer 和UIPinchGestureRecognizer
---

```
当同时使用pan和pin手势时假如冲突，需要加入下面方法
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer {
    return  YES;
}
```

