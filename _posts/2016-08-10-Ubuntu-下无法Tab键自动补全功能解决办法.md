---
layout: post
title: Ubuntu 下无法Tab键自动补全功能解决办法
---

在Ubuntu下 使用Tab键报错：cannot create temp file for here-document: no space left on device

解决：

```
rm -rf /var/log
rm -rf /tmp/
```

