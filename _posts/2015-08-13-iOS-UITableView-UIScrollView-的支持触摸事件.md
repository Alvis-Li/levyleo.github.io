---
layout: post
title: iOS UITableView UIScrollView 的支持触摸事件
---

在使用了 UITableView 或UIScrollView的controller 里无法响应触摸事件touch事件， 自定义tableView、scrollView

```
#import <UIKit/UIKit.h>
 
@interface touchScrollView : UIScrollView
 
@end
```

```
#import "touchScrollView.h"
 
@implementation touchScrollView
 
- (id)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        // Initialization code
    }
    return self;
}
 
-(void) touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event{
    [super touchesBegan:touches withEvent:event];
    if ( !self.dragging )
    {
        [[self nextResponder] touchesBegan:touches withEvent:event];
    }
}
 
-(void) touchesEnded:(NSSet *)touches withEvent:(UIEvent *)event{
    [super touchesEnded:touches withEvent:event];
    if ( !self.dragging )
    {
        [[self nextResponder] touchesEnded:touches withEvent:event];
    }
}
 
-(void) touchesCancelled:(NSSet *)touches withEvent:(UIEvent *)event
{
    [super touchesEnded:touches withEvent:event];
    if(!self.dragging)
    {
        [[self nextResponder] touchesCancelled:touches withEvent:event];
    }
}
 
-(void) touchesMoved:(NSSet *)touches withEvent:(UIEvent *)event
{
    [super touchesMoved:touches withEvent:event];
    if(!self.dragging)
    {
        [[self nextResponder] touchesMoved:touches withEvent:event];
    }
}
```

