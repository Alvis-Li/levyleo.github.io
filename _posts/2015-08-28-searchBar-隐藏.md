---
layout: post
title: searchBar 隐藏
---

```
searchBar 隐藏

CGRect newBounds = self.tableView.bounds;

    newBounds.origin.y = newBounds.origin.y + _headerSearchBar.bounds.size.height;

    self.tableView.bounds = newBounds;

 

但是这样隐藏后 当cell个数较少时会出现,push到下一界面再pop回到当前界面searchBar 会重新出现, 

动态设置TableFooterView, 使cell的总高度加上TableFooterView的高度大于tableView的高度,就可以避免这一现象.

 

-(void)tableViewFooterViewHight{

    

    CGRect rect = self.tableView.tableFooterView.frame;

    UIView * view = self.tableView.tableFooterView;

    self.tableView.tableFooterView = nil;

    

    CGFloat hight = 0;

    hight = 62+50;

    

    ABAuthorizationStatus authStatus =

    ABAddressBookGetAuthorizationStatus();

    if(authStatus != kABAuthorizationStatusAuthorized){

        hight +=40;

    }else{

        hight += 0.0f;

    }

    

    hight += 30.f;

    hight += ( _data.count + 1 )*65.f;

    

    hight = CGRectGetHeight(self.tableView.frame) - hight;

    

    rect.size.height = hight;

    view.frame = rect;

    

    self.tableView.tableFooterView = view;

    

}
```

