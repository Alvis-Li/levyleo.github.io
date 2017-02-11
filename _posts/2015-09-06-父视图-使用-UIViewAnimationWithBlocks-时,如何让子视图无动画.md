---
layout: post
title: 父视图 使用 UIViewAnimationWithBlocks 时,如何让子视图无动画
---

```
tableView使用 UIViewAnimationWithBlocks 时 上面的cell也会一起出现动画, 所以在设置cell的时候 添加 

[UIView performWithoutAnimation:^{

cell 设置 , 初始化

}];
```

