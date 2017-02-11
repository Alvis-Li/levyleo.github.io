---
layout: post
title: searchDisplayController 时引起的数组越界
---

当  [searchDisplayController.searchResultsTableView setSeparatorStyle:UITableViewCellSeparatorStyleNone] 时， 发送了崩溃

错误提示如下：

```
Terminating app due to uncaught exception 'NSRangeException', reason: '*** -[__NSArrayI objectAtIndex:]: index 1 beyond bounds [0 .. 0]'

*** First throw call stack:

(

0   CoreFoundation                      0x000000010c6c6c65 __exceptionPreprocess + 165

1   libobjc.A.dylib                     0x000000010c35fbb7 objc_exception_throw + 45

2   CoreFoundation                      0x000000010c5bd17e -[__NSArrayI objectAtIndex:] + 190

3   UIKit                               0x000000010d230fd2 -[UITableViewDataSource tableView:indentationLevelForRowAtIndexPath:] + 106

4   UIKit                               0x000000010cdfb1b9 __53-[UITableView _configureCellForDisplay:forIndexPath:]_block_invoke + 1711
```

查了好久才查到原因： 在错误log中有提示

```
3   UIKit                               0x000000010d230fd2 -[UITableViewDataSource tableView:indentationLevelForRowAtIndexPath:] + 106
```

解决方法：

```
-(NSInteger)tableView:(UITableView *)tableView indentationLevelForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 0;
}
```

