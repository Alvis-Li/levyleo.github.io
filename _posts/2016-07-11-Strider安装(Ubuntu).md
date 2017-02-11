---
layout: post
title: Strider安装(Ubuntu)
---

安装：

```
git clone https://github.com/Strider-CD/strider.git && cd strider
 
nam install
```

 然而还是出现一大波错误

```

3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
npm WARN deprecated jade@0.35.0: Jade has been renamed to pug, please install the latest version of pug instead of jade
npm WARN deprecated swig@0.14.0: v1.0.0 is a complete rewrite of Swig from the ground up. Previous versions are no longer supported
npm WARN deprecated lodash@1.3.1: lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0.
npm WARN deprecated transformers@2.1.0: Deprecated, use jstransformer
npm WARN deprecated lodash@2.2.1: lodash@<3.0.0 is no longer maintained. Upgrade to lodash@^4.0.0.
npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@1.2.3: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
npm WARN deprecated minimatch@0.2.14: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated graceful-fs@3.0.8: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
npm WARN deprecated wrench@1.5.9: wrench.js is deprecated! You should check out fs-extra (https://github.com/jprichardson/node-fs-extra) for any operations you were using wrench for. Thanks for all the usage over the years.
npm WARN deprecated graceful-fs@2.0.3: graceful-fs v3.0.0 and before will fail on node releases >= v7.0. Please update to graceful-fs@^4.0.0 as soon as possible. Use 'npm ls graceful-fs' to find it in the tree.
npm WARN deprecated minimatch@1.0.0: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated npmconf@2.1.2: this package has been reintegrated into npm and is now out of date with respect to npm
npm WARN deprecated minimatch@0.3.0: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated jade@0.26.3: Jade has been renamed to pug, please install the latest version of pug instead of jade
npm WARN prefer global n@2.1.2 should be installed with -g
npm WARN prefer global npm@2.15.9 should be installed with -g
npm WARN prefer global bower@1.3.12 should be installed with -g
```

 继续安装

```
sudo npm install -g pug swig lodash  jstransformer minimatch graceful-fs fs-extra graceful-fs npmconf n npm bower

```

 OK，开始运行

```
npm start
 
或者
 
bin/strider
```

 运行成功但是有警告：

```
{ [Error: Cannot find module '../build/Release/bson'] code: 'MODULE_NOT_FOUND' }
js-bson: Failed to load c++ bson extension, using pure JS version
1290 forked
{ [Error: Cannot find module '../build/Release/bson'] code: 'MODULE_NOT_FOUND' }
js-bson: Failed to load c++ bson extension, using pure JS version
```

 解决方法；见[Strider安装(mac)](http://www.cnblogs.com/levy/p/5660859.html)

安装后添加用户：

```
strider addUser
或者
bin/strider addUser
```

