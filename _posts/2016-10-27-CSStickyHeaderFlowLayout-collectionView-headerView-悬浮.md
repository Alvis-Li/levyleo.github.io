---
layout: post
title: CSStickyHeaderFlowLayout collectionView headerView 悬浮
---

github:[https://github.com/levyleo/CSStickyHeaderFlowLayout](https://github.com/levyleo/CSStickyHeaderFlowLayout)

iOS 10 使用时会出现崩溃:[https://github.com/CSStickyHeaderFlowLayout/CSStickyHeaderFlowLayout/issues/144](https://github.com/CSStickyHeaderFlowLayout/CSStickyHeaderFlowLayout/issues/144)
Swift 解决方法:

```swift
override func viewDidLoad() {    
super.viewDidLoad()
if #available(iOS 10.0, *) {   
self.collectionView?.isPrefetchingEnabled = false   
} else {   
//Fallback on earlier versions   
}}
```

但是在OC里面 isPrefetchingEnabled 是不可访问的,但是可以使用KVC

```
[_collectionView setValue:@NO forKey:@"_prefetchingEnabled"];
```

问题解决!