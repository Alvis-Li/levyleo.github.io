---
layout: post
title: Mac 上批量分割文件
---

split 分割文件命令 eg: `split -l 10000000 data1.csv spfile.`
其他命令可以再网上查询
shell 循环命令可以使用 for 循环

```
for((i=0;i<10;i++));do echo $i; done
```

也可使用

```
for i in `ls`;do echo $i; done
```

第二种方法可以遍历文件夹中的文件,所有根据第二种方法,改写

```
for i in `ls`; do `split -l 1 $i $i.`; done;
```

分割一个目录下的所有文件,完成.要加其他过滤条件可以自行Google

```
sed -ie 's/,$//g' /Users/XXX.csv
```

-i 直接修改源文件, -e 多次编辑
's/,$//g'  s为替换, $表示一行的结尾 g 是全局
's/,$/'中的//中间的',$'为要替换的字符,在这里用的是正则
'//g'中的//中间为空,表示要替换成空字符