SDWebimage 以数据流格式下载图片不成功解决办法

服务器代码(node.js + restify):

```
...
 var stream = fs.createReadStream(file.path);
stream.pipe(res);
...
```



正常情况下使用SDWebimage 设置图片使用sd_setImageWithUrl 方法即可,但是在这种情况下无法获取到数据,解决办法可以从服务器和app方面入手:

1. (参照: [SDWebImage Error Domain=NSURLErrorDomain错误](http://www.it610.com/article/2136195.htm) ) (更详细的请查看原文)

```
restify默认情况下不支持图片类型的mime头（image/*） 
把mime类型image/*加上即可
```

2. (参照:[Ios SDWebimage Error Domain=NSURLErrorDomain Code=406 报错](http://www.zhimengzhe.com/IOSkaifa/53029.html)) (更详细的请查看原文)

```
第一种方法:修改SDWebImage代码
_HTTPHeaders = [NSMutableDictionary dictionaryWithObject:@"image/webp,image/*;q=0.8" 
                                                  forKey:@"Accept"];
                                                  
第二种方法:添加header代码
[SDWebImageDownloader.sharedDownloader setValue:@"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8"
                                     forHTTPHeaderField:@"Accept"];
               
```

YYKit同理

```
 [[YYWebImageManager sharedManager] setHeaders:    @{@"Accept":@"text/html,application/xhtml+xml,application/xml;q=0.9,image/*,*/*;q=0.8"}];
```

