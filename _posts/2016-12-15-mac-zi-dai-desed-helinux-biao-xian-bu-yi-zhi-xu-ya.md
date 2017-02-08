---
layout: post
title: mac自带的sed和linux表现不一致， 需要安装gnu-sed (转载)
---

[原文](http://blog.csdn.net/wk3368/article/details/50876808)
1.本来想把逗号替换成换行，结果不行。
\(echo "a,b,c,d" |sed  's/,/\n/g' anbncnd  网上查了一下，原来是mac的sed对\n的处理和Linux不一样，
详见:http://superuser.com/questions/307165/newlines-in-sed-on-mac-os-x    解决办法： 1.brew
 install gnu-sed --with-default-names 2.vi ~/.zshrc export 
PATH="/usr/local/opt/gnu-sed/libexec/gnubin:\)PATH" 3.source ~/.zshrc 
或者新开窗口，让设置生效   再试，就可以了！ $echo "a,b,c,d" |sed  's/,/\n/g' a b c d    
2.sed -i 替换文件内容的时候，如果想在原始文件上替换，不想生成额外的副本，就需要在正则表达式的前面多加一个""
sed -i "" "s/aaa/bbb/g"  a.txt      直接替换a.txt
或者
sed -i_bak "s/aaa/bbb/g"  a.txt  会替换a.txt 但同时生成a_bak.txt
 
如果换成gnu-sed ，直接
sed -i "s/aaa/bbb/g"  a.txt 
即可，跟linux上的表现一致!