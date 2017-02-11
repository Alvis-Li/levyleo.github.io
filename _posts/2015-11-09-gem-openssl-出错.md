---
layout: post
title: gem openssl 出错
---

```
Unable to require openssl, install OpenSSL and rebuild ruby (preferred) or use non-HTTPS sources

```

1.

```
gem sourc
```

查看source: 将https://替换成http://

```
gem source -r https://ruby.taobao.org/
gem source -a http://rubygems.org/
```

