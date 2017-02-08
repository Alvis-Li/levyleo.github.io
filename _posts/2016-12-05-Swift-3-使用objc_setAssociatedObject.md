---
layout: post
title: Swift 3 使用objc_setAssociatedObject 
---

起初使用runtime添加属性的时候照着OC习惯

```
objc_setAssociatedObject(self, "sharedInstance","sharedInstance",.OBJC_ASSOCIATION_RETAIN_NONATOMIC)print(objc_getAssociatedObject(self, "sharedInstance"))
```

结果无论如何都添加不成功,
后来参照[此文](http://www.jianshu.com/p/53abf1703905)

```
let key: UnsafeRawPointer! = UnsafeRawPointer.init(bitPattern: "sharedInstance".hashValue)objc_setAssociatedObject(self,key, "sharedInstance", .OBJC_ASSOCIATION_RETAIN_NONATOMIC)print(objc_getAssociatedObject(self, key))
```

成功,mark一下