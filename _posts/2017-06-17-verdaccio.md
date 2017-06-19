---
layout: post
title: verdaccio搭建私有npm服务器
---

[verdaccio](https://github.com/levyleo/verdaccio)

安装：`npm install -g verdaccio`

启用：`verdaccio`

```
 warn --- config file  - ~/.config/verdaccio/config.yaml //配置文件的路径
 warn --- http address - http://0.0.0.0:3873/ //默认的端口号为4873,已经在配置文件中修改
```

配置：`npm set registry http://localhost:3873/` 
设置镜像为本地镜像,npm 进行安装操作时会先对本地进行查找，没有找到的会去npm公共镜像查找。

HTTPS: 
```
# if you use HTTPS, add an appropriate CA information
# ("null" means get CA list from OS)
$ npm set ca null
```

修改配置：`vi ~/.config/verdaccio/config.yaml`

```
# 修改监听的端口号
listen: 0.0.0.0:3873
```

保存重启即可

更多 [配置1](https://github.com/levyleo/verdaccio/blob/master/conf/default.yaml)   [配置2](https://github.com/levyleo/verdaccio/blob/master/conf/full.yaml)

创建npm包上传到自己的服务器中

同[上传自己创建的npm包](http://www.levyleo.cn/2017/06/16/npm.html)中的用法，假如已经设置了`npm set registry http://localhost:3873/` 完全相同，即可将npm包上传到自己的服务器，如果需要在每个命令后面添加` '--registry http://127.0.0.1:3873'`