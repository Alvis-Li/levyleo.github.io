---
layout: post
title: 根据headerView位置改变headerView颜色(collectionView/tableview)
---

滑动时,tableview中的headerView 的frame不断改变,collectionView的headerView的center不断改变.
so
tableview:

```
- (void) setFrame: (CGRect) frame {
	[super setFrame: frame];
	CGRect rect = [self.superview convertRect: frame toView: [UIApplication sharedApplication].keyWindow];
	if (rect.origin.y > 65 && rect.size.height > 0 && ![self.backgroundColor isEqual: [UIColor whiteColor]]) {
		[self setBackgroundColor: [UIColor whiteColor]];
	} else if (rect.origin.y < 65 && rect.size.height > 0 && ![self.backgroundColor isEqual: UIColorFromRGB(0xff3333)]) {
		[self setBackgroundColor: UIColorFromRGB(0xff3333)];
	}
}

```

collectionView:

```
#import "CollectionReusableView.h"
@interface CollectionReusableView() {
	CGFloat centerY;
}
@end @implementation CollectionReusableView - (void) setFrame: (CGRect) frame {
	[super setFrame: frame];
	centerY = self.frame.origin.y + self.frame.size.height / 2.0;
} - (void) setCenter: (CGPoint) center {
	[super setCenter: center];
	if (center.y < centerY + 1 && ![self.backgroundColor isEqual: [UIColor whiteColor]]) {
		[self setBackgroundColor: [UIColor whiteColor]];
	} else if (center.y > centerY + 1 > 0 && ![self.backgroundColor isEqual: [UIColor redColor]]) {
		[self setBackgroundColor: [UIColor redColor]];
	}
}
@end
```

