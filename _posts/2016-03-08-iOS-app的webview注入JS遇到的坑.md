---
layout: post
title: iOS app的webview注入JS遇到的坑
---

webview使用JSContext 向网页js注入时时机要选为网页加载完成后即放在

```
-(void)webViewDidFinishLoad:(UIWebView *)webView 方法

```

```
-(void)webViewDidFinishLoad:(UIWebView *)webView{
     
    context = [self.webView  valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
    __block __weak DetailWebController *weakSelf = self;
     
    context[@"ios_follow"] = ^() {
        NSArray *args = [JSContext currentArguments];
        BOOL toBool = [((JSValue *)args.firstObject) toBool];
        NSString *toString = [((JSValue *)args.lastObject) toString];
        if (toBool) {
            dispatch_async(dispatch_get_main_queue(), ^{
                [weakSelf getChaseDramaWithID:toString];
            });
        }else{
            dispatch_async(dispatch_get_main_queue(), ^{
                [weakSelf getCancelChaseDramaWithID:toString];
            });
        }
    };
    context[@"ios_score"] = ^() {
        NSArray *args = [JSContext currentArguments];
        JSValue * value = args.firstObject;
        NSInteger score = [value toUInt32];
        [weakSelf setScoreWithScore:score];
    };
}
```

