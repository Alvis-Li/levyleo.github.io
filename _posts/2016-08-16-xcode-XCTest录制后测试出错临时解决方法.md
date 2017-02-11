---
layout: post
title: xcode XCTest录制后测试出错临时解决方法
---

Xcode 使用 XCTest 做自动化测试,录制后在run UITest 的时候总是莫名报错,后来在方法执行前添加sleep()后,没有再出现错误,可能是xcode的一处BUG.