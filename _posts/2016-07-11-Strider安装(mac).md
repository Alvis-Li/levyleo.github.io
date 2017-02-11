---
layout: post
title: Strider安装(mac)
---

安装：

```
sudo npm install -g strider
```

安装时实现一些⚠️

```
npm WARN deprecated jade@0.35.0: Jade has been renamed to pug, please install the latest version of pug instead of jade
npm WARN deprecated swig@0.14.0: v1.0.0 is a complete rewrite of Swig from the ground up. Previous versions are no longer supported
npm WARN deprecated lodash@1.3.1: lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0.
npm WARN deprecated transformers@2.1.0: Deprecated, use jstransformer
npm WARN deprecated lodash@2.2.1: lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0.
npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@1.2.3: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
npm WARN deprecated minimatch@0.2.14: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@3.0.8: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
npm WARN deprecated has-color@0.1.7: Renamed to supports-color. If you're using chalk, upgrade to the latest version. https://github.com/chalk/supports-color
```

```
> fsevents@0.3.0 install /usr/local/lib/node_modules/strider/node_modules/fsevents
> node-gyp rebuild
 
  CXX(target) Release/obj.target/fse/fsevents.o
In file included from ../fsevents.cc:6:
../node_modules/nan/nan.h:342:68: error: too many arguments to function call, expected at
      most 2, have 4
    return v8::Signature::New(v8::Isolate::GetCurrent(), receiver, argc, argv);
```

 更新依赖包：

```
sudo npm install -g pug lodash swig jstransformer minimatch graceful-fs minimatch supports-color

```

```
sudo npm install -g fsevents
```

  启动strider

```
strider
```

又出现了警告：

```
{ Error: Cannot find module '../build/Release/bson'
    at Function.Module._resolveFilename (module.js:440:15)
    at Function.Module._load (module.js:388:25)
    at Module.require (module.js:468:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (/usr/local/lib/node_modules/strider/node_modules/mongoose/node_modules/bson/ext/index.js:15:10)
    at Module._compile (module.js:541:32)
    at Object.Module._extensions..js (module.js:550:10)
    at Module.load (module.js:458:32)
    at tryModuleLoad (module.js:417:12)
    at Function.Module._load (module.js:409:3) code: 'MODULE_NOT_FOUND' }
js-bson: Failed to load c++ bson extension, using pure JS version
```

 网上搜了一下说要修改bson的index.js文件：

找到 npm 的module mongodb ..node_modules\mongodb\node_modules\bson\ext\index.js

并在catch块改变bson的js本版路径：

bson = require('../build/Release/bson');

变成

bson = require('../browser_build/bson');

 

再次启动strider 成功。

 假如安装在本地，直接访问127.0.0.1:3000 即可打开

 

补充：

安装后添加用户：

```
strider addUser
或者
bin/strider addUser
```

