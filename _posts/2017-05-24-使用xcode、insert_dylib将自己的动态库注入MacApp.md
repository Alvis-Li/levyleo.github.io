---
layout: post
title: 使用xcode、insert_dylib将自己的动态库注入MacApp
---

insert_dylib 依赖参照:https://github.com/Tyilo/insert_dylib

下载后编译,或者直接[下载](http://www.levyleo.cn/source/insert_dylib):

此篇文章和[如何愉快地在Mac上刷朋友圈-图解](http://www.levyleo.cn/2017/03/14/如何愉快地在Mac上刷朋友圈-图解.html)类同

用的app是[Hopper 替换Mac app中的文字](http://www.levyleo.cn/2017/05/24/Hopper-替换Mac-app中的文字.html)生成的app

准备条件：

1、JRSwizzle：可使用pod安装，也可以直接拿文件来用 添加在创建项目时自动生成的文件中，我创建的项目名称为test_p，所以加在test_p.h中

```
pod 'JRSwizzle'
```

2、一些将要用到的宏 也需要添加在test_p.h中

```

#define CBGetClass(classname) objc_getClass(#classname)
#define CBRegisterClass(superclassname, subclassname) { Class class = objc_allocateClassPair(CBGetClass(superclassname), #subclassname, 0);objc_registerClassPair(class); }
#define CBHookInstanceMethod(classname, ori_sel, new_sel) { NSError *error; [CBGetClass(classname) jr_swizzleMethod:ori_sel withMethod:new_sel error:&error]; if(error) NSLog(@"%@", error); }
#define CBHookClassMethod(classname, ori_sel, new_sel) { NSError *error; [CBGetClass(classname) jr_swizzleClassMethod:ori_sel withClassMethod:new_sel error:&error]; if(error) NSLog(@"%@", error); }
#define CBGetInstanceValue(obj, valuename) object_getIvar(obj, class_getInstanceVariable([obj class], #valuename))
#define CBSetInstanceValue(obj, valuename, value) object_setIvar(obj, class_getInstanceVariable([obj class], #valuename), value)
```



3、对应的script

```
#!/bin/bash
app_name="test" #要注入的app名字
framework_name="test_p" #自己创建的动态库项目名字
app_bundle_path="~/Desktop/${app_name}.app/Contents/MacOS" #要注入的app中的二进制文件路径
app_executable_path="${app_bundle_path}/${app_name}"
app_executable_backup_path="${app_executable_path}_"
framework_path="${app_bundle_path}/${framework_name}.framework"
# 备份app原始可执行文件
if [ ! -f "$app_executable_backup_path" ]
then
cp "$app_executable_path" "$app_executable_backup_path"
fi
cp -r "${BUILT_PRODUCTS_DIR}/${framework_name}.framework" ${app_bundle_path}
insert_dylib --all-yes "${framework_path}/${framework_name}" "$app_executable_backup_path" "$app_executable_path"
```

4、[app 对应的一些头文件](http://www.levyleo.cn/2017/05/24/class-dump-获取别人app的头文件.html)

5、创建Main.h入口文件

```
Main.h
//
//  Main.h
//  test_p
//
//  Created by simple on 2017/5/24.
//  Copyright © 2017年 simple. All rights reserved.
//

#import "test_p.h"
```

```
//
//  Main.m
//  test_p
//
//  Created by simple on 2017/5/24.
//  Copyright © 2017年 simple. All rights reserved.
//

#import "Main.h"
#import "ViewController.h" //此文件是使用class-dump从test.app中提取出来的头文件

@implementation NSObject (MacWeChatTimeLinePlugin)

-(void)test:(id)arg1{
    NSLog(@"aaa");
}

@end


static void __attribute__((constructor)) initialize(void) { //初始化方法
    NSLog(@"++++++++ loaded ++++++++");
    CBHookInstanceMethod(ViewController, @selector(click:), @selector(test:));//将ViewController中的click方法指向test
}
```

[测试demo](https://github.com/levyleo/insert_dylibDemo)

