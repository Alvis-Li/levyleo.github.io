---
layout: post
title: CircleLayout
---

```
CircleLayout
 
https://developer.apple.com/library/ios/samplecode/CircleLayout/Introduction/Intro.html#//apple_ref/doc/uid/DTS40012315
 
 
//1.
 
[self.collectionView performBatchUpdates:^{
    [self.collectionView reloadItemsAtIndexPaths:@[indexPath]];
} completion:^(BOOL finished) {
 
}];
 
//这个方法会在第一个 block 中处理 insert/delete/reload/move 等操作，等操作完成之后会执行第二个block。
 
//2.自定义UICollectionViewLayout 类 CircleLayout
_viewController = [[ViewController alloc] initWithCollectionViewLayout:[[CircleLayout alloc] init]];
 
//(1)继承父类方法
-(void)prepareLayout
{
    [super prepareLayout];
    //在此方法中初始化此类的属性
    CGSize size = self.collectionView.frame.size;
   _cellCount = [[self collectionView] numberOfItemsInSection:0];
   _center = CGPointMake(size.width / 2.0, size.height / 2.0);
   _radius = MIN(size.width, size.height) / 2.5;
}
 
//(2)返回CollectionView 内容页的大小
-(CGSize)collectionViewContentSize
{
    return [self collectionView].frame.size;
}
 
//(3)在rect区域内 返回每个cell的LayoutAttributes
-(NSArray*)layoutAttributesForElementsInRect:(CGRect)rect
{
    NSMutableArray* attributes = [NSMutableArray array];
    for (NSInteger i=0 ; i < self.cellCount; i++) {
        NSIndexPath* indexPath = [NSIndexPath indexPathForItem:i inSection:0];
        [attributes addObject:[self layoutAttributesForItemAtIndexPath:indexPath]];
    }
    return attributes;
}
//(4) 确定 attributes 的size 和 center
- (UICollectionViewLayoutAttributes *)layoutAttributesForItemAtIndexPath:(NSIndexPath *)path
{
    UICollectionViewLayoutAttributes* attributes = [UICollectionViewLayoutAttributes layoutAttributesForCellWithIndexPath:path];
    attributes.size = CGSizeMake(ITEM_SIZE, ITEM_SIZE);
    attributes.center = CGPointMake(_center.x + _radius * cosf(2 * path.item * M_PI / _cellCount),
                                    _center.y + _radius * sinf(2 * path.item * M_PI / _cellCount));
    return attributes;
}
```

