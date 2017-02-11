---
layout: post
title: Strider-test 相关配置
---

package.json

```
{
  "name": "test-node",
  "version": "0.0.0",
  "description": "A test repo for node support",
  "main": "index.js",
  "scripts": {
    "test": "node index.js",
    "deploy":"node index.js"
  },
  "repository": {
    "type": "git",
    "url": "git://gitlab.com:levyleo/strider-test.git"
  },
  "keywords": [
    "test",
    "repo",
    "strider"
  ],
  "author": "levy <lqcqc@sina.com>",
  "license": "BSD-2-Clause",
  "bugs": {
    "url": "https://gitlab.com/levyleo/strider-test/issues"
  },
  "homepage": "https://gitlab.com/levyleo/strider-test"
}
```

 index.js

```
console.log('水水水水');
```

