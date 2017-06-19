---
layout: post
title: 将Node.js项目编译成可执行文件
---

[**Node.js Compiler**](https://github.com/pmq20/node-compiler): 将Node.js应用程序编译成单个可执行文件。

MacOS安装方法:

1、SquashFS: `brew install squashfs`

2、gcc and make: 需要借助xcode

3、Python： 2.6 or 2.7

4、GNU：3.81 [可参照此链接安装](http://openwares.net/linux/mac_os_x_install_gnu_tools.html)

或者将下面内容写到脚本里：（此处开头#!/bin/bash 和上面链接里的不同，mac中自带bash可以直接使用系统自带的bash，系统自带的bash路径为/bin/bash）

```
#!/bin/bash

# add source
brew tap homebrew/dupes

# GNU Coreutils
brew install coreutils

# build tools
brew install autoconf
brew install m4
brew install make

# misc
brew install binutils
brew install diffutils
brew install ed --default-names
brew install findutils --default-names
brew install gawk
brew install gnu-indent --default-names
brew install gnu-sed --default-names
brew install gnu-tar --default-names
brew install gnu-which --default-names
brew install gnutls --default-names
brew install grep --default-names
brew install gzip
brew install screen
brew install watch
brew install wdiff --with-gettext
brew install wget

# third party 
brew install git
brew install less
brew install openssh --with-brewed-openssl
brew install python3 --with-brewed-openssl
brew install rsync
brew install unzip
brew install vim --override-system-vi

```

5、nodec:

```
curl -L https://sourceforge.net/projects/node-compiler/files/v1.0.0/nodec-darwin-x64/download > nodec
chmod +x nodec
./nodec 
```  

也可将nodec 放到'/usr/local/bin/' 这样就可以全局使用
