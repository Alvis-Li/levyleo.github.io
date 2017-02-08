---
layout: post
title: Mac中设置别名 (转载)
---

[原文](http://blog.csdn.net/wanghantong/article/details/38424091)

在macboo k中设置别名方便使用：

具体方法如下：

[sql] view plain copy
在CODE上查看代码片派生到我的代码片

```
cd ~  
vim .bashrc  
alias xxx=****  
例:  
alias mysqladmin=/usr/local/mysql/bin/mysqladmin  
退出保存  

source .bashrc  -----  使其生效</span>  

```

如果还不行：

打开

~/.bash_profile

加入

source ~/.bashrc