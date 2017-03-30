---
layout: post
title: Jekyll https css no loading on http site
---

摘自:[stackoverflow](http://stackoverflow.com/questions/39481319/jekyll-http-css-no-loading-on-https-site)

When linking a resource to your document, you don't need to explicitly set `http:` or `https:`, only double slashes `//` will work fine, it will base on the current protocol of the page:

```
<link rel="stylesheet" href="//wys35.github.io/css/main.css">
```

Try setting `url` in `_config.yml` to `"//wys35.github.io"`



简单的说jekyll搭建的blog,在http下正常,在https下css无法加载,错误代码:SSL_ERROR_BAD_CERT_DOMAIN

查看调试器发现在http网页调用了https的css, 当然是因为对jekyll还不很熟悉.

_config.yml 原本配置: url: https://www.levyleo.cn  改为:url: //www.levyleo.cn 即可