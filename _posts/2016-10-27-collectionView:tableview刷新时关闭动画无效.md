---
layout: post
title: collectionView/tableview刷新时关闭动画无效
---

collectionView/tableview reloadSections/reloaddata时去掉动画无效时可以尝试使用

```
[UIView performWithoutAnimation:^{    [_collectionView reloadSections:[NSIndexSet indexSetWithIndex:2]];    //刷新操作 }];
```