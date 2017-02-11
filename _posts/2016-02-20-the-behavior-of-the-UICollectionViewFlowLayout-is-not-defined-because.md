---
layout: post
title: the behavior of the UICollectionViewFlowLayout is not defined because
---

**the behavior of the UICollectionViewFlowLayout is not defined because:**

 

the item height must be less than the height of the UICollectionView minus the section insets top and bottom values, minus the content insets top and bottom values.



**The relevant UICollectionViewFlowLayout instance is , and it is attached to ; layer = ; contentOffset: {0, 0}; contentSize: {0, 125}> collection view layout: .**



**Make a symbolic breakpoint at UICollectionViewFlowLayoutBreakForInvalidSizes to catch this in the debugger.**

```
- (void)viewDidLoad {
    [super viewDidLoad];

    
    self.automaticallyAdjustsScrollViewInsets = NO;
}
```

