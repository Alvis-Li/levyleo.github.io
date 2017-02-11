---
layout: post
title: 淘宝的ruby镜像已无人维护,使用ruby-china的RubyGems镜像
---

淘宝的镜像已经无人维护了，参考

[https://ruby-china.org/topics/29250](https://ruby-china.org/topics/29250)

[https://gems.ruby-china.org/](https://gems.ruby-china.org/)

使用新的镜像

```
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/$ gem sources -l
*** CURRENT SOURCES ***
 
https://gems.ruby-china.org# 请确保只有 gems.ruby-china.org$ gem install rails
```

 涉及到代理服务器的话，这么搞：

```
>>SET http_proxy=>>SET https_proxy=
>>gem install something
or
>>gem list -p http://user:pwd@ip_or_host:8080 -r
```

