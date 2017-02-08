---
layout: post
title: rvm pod gem 等常用命令
---

### rvm

安装:
`curl -L https://get.rvm.io | bash -s stable --autolibs=enabled [--ruby] [--rails] [—trace]`

```
$ curl -L get.rvm.io | bash -s stable 
$ source ~/.bashrc  
$ source ~/.bash_profile 
rvm -v 

```

### 用RVM升级Ruby     

```
1. #查看当前ruby版本  
2. $ ruby -v  
3. ruby 1.8.7  
4. #列出已知的ruby版本  
5. $ rvm list known  
6. #安装ruby 1.9.3  
7. $ rvm install 1.9.3 
8. 卸载
9. rvm remove 1.9.3
卸载RVM:   rvm implode
$ cd ~ ; sudo rm -rf .rvm .rvmrc   /etc/rvmrc ;gem uninstall rvm

```

### homebrew 

安装:   

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

卸载:

```
cd `brew --prefix`
rm -rf Cellar
brew prune
rm `git ls-files`
rm -r Library/Homebrew Library/Aliases Library/Formula Library/Contributions
rm -rf .git
rm -rf ~/Library/Caches/Homebrew

```

更新:

```
brew update

brew update —system

安装， 如：brew install unrar

卸载， 如：brew uninstall unrar


```

### Homebrew安装Git

`brew install git `

### 卸载git: 

```
rm -rf /usr/local/git
rm /etc/paths.d/git
rm /etc/manpaths.d/git

sudo rm -rf /usr/local/git /usr/bin/git /etc/paths.d/git /etc/manpaths.d/git


```

### gem

```
#查看gem源
gem sources
# 删除默认的gem源 
gem sources --remove http://rubygems.org/
# 增加taobao作为gem源 
gem sources -a http://ruby.taobao.org/
# 查看当前的gem源
gem sources
*** CURRENT SOURCES ***
http://ruby.taobao.org
# 请确保只有 ruby.taobao.org
# 清空源缓存
gem sources -c
# 更新源缓存
gem sources -u


```

### pod 

```
安装: gem 应该是ruby自带的ruby包管理器 
sudo gem install cocoapods
卸载:
sudo gem uninstall cocoapods

```

### openssl

```
升级:
http://openssl.org/

    •   ./Configure darwin64-x86_64-cc
    •   make
    •   make test
    •   sudo make install
    •   export PATH="/usr/local/ssl/bin:$PATH"



$ which openssl
/usr/bin/openssl
$ /usr/bin/openssl version
OpenSSL 0.9.8x 10 May 2012
$ /usr/local/ssl/bin/openssl version
OpenSSL 1.0.1e 11 Feb 2013

```

### ruby:

```
wget http://cache.ruby-lang.org/pub/ruby/2.2/ruby-2.2.2.tar.gz
tar xfvz ruby-2.2.2.tar.gz
cd ruby-2.2.2/
./configure
make
sudo make install

安装make 命令
https://developer.apple.com/downloads/index.action?name=for%20Xcode%20- 
下载 https://developer.apple.com/downloads/index.action?name=for%20Xcode%20-# 

command_line_tools_for_xcode_june_2012.dmg

```

### Wget 安装

```
curl -O http://ftp.gnu.org/gnu/wget/wget-1.13.4.tar.gz
tar -xzvf wget-1.13.4.tar.gz
cd wget-1.13.4
./configure --with-ssl=openssl
make
sudo make install
```

