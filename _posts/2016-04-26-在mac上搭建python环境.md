---
layout: post
title: 在mac上搭建python环境
---

原文出处:http://blog.justbilt.com/2014/07/02/setup_python_on_mac/

```
这两天重新搞了下python的环境，发现好多地方还是容易忘记，因此有了这篇文章，以后方便查看。
 
一. 安装python
 
其实mac自带的python完全够用, 这一步可以跳过. – by Bin
 
mac系统自带了一个python的执行执行环境，但为了获取最新版的python，我们需要重新安装python。这里有两种方案安装：
 
1.homebrew
 
brew install python
这个方案比较简单,如果出错的话可以给前面加sudo试试,这个安装的python可能不是最新版.
 
2.从官网下载安装
大家可以从https://www.python.org/download下载安装最新版的python,安装比较无脑,一路按下去就OK,缺点是以后升级,卸载都得自己维护.
 
这两个方法安装的python的位置是不一样的,大家可以用:
 
which python
来查看安装位置.安装完成后在终端中键入python来验证安装是否成功.
 
二. 安装pip
 
这里好多文章中说要先安装easy_install, 其实是不用的.
 
1.我们先获取pip安装脚本:
 
wget https://bootstrap.pypa.io/get-pip.py
如果没有安装wget可以去这里将所有内容复制下来,新建get-pip.py文件,将内容拷进去就OK了.
 
2.安装pip
 
sudo python get-pip.py
用python执行刚才获取的脚本,这里sudo可以选择使用,若遇到类似这个报错则必须加sudo:
 
Exception:
 
Traceback (most recent call last):
 
...
 
OSError: [Errno 13] Permission denied: 'XXX/pip-0.7.2-py2.7.egg/EGG-INFO/dependency_links.txt'
 
Storing debug log for failure in /Users/bilt/.pip/pip.log
安装成功后可以在终端中键入pip来检测,如果不行重启终端后尝试.
 
3.修改pip源
 
在天朝,由于功夫网的原因,使用pip安装一些模块会特别慢甚至无法下载,因此我们需要修改pip的源到国内的一些镜像地址,特别感谢国内无私奉献的组织~
 
首先进入HOME路径:
 
cd ~
创建.pip目录:
 
mkdir .pip
创建pip.conf文件:
 
touch pip.conf
大家可以用自己喜欢的编辑器打开pip.conf文件,我现在使用的时v2ex的源,所以添加:
 
[global]
index-url = http://pypi.v2ex.com/simple
大家可以把index-url的值设置为自己实际源的地址.
 
至此pip源修改成功,以后使用pip安装模块时都会从这个源去下载安装,大家可以自行测试一下.
 
三. 其他模块安装
 
1.Pillow/PIL
 
想用python处理图片,自然少不了PIL这个模块, 由于PIL长期没有更新了, 所以有了Pillow这个模块, 依赖于PIL, 新版的pip安装后会自带Pillow, 但是好像没有zlib模块, 所以会报错:
 
File "/Library/Python/2.7/site-packages/PIL/Image.py", line 1105, in paste
    im.load()
 
  File "/Library/Python/2.7/site-packages/PIL/ImageFile.py", line 190, in load
 
    d = Image._getdecoder(self.mode, d, a, self.decoderconfig)
 
  File "/Library/Python/2.7/site-packages/PIL/Image.py", line 389, in _getdecoder
 
    raise IOError("decoder %s not available" % decoder_name)
 
IOError: decoder zip not available
因此我们需要手动重新安装:
 
sudo pip install -U Pillow
```

