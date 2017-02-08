---
layout: post
title: 部分shell命令(自用)
---

```
split -l 10000000 data1.csv spfile. //分割文件


for i in `ls`; do `split -l 1 $i $i.`; done; //批量分割文件


sed -ie 's/,$//g' /Users/simple/Desktop/py/lanzou9.csv


for i in `ls`; do `sed -ie 's/,$//g' $i`; done;


head -n 1 _.csv 读取文件头几行


cut -c 1-4 _.csv 按行读取文件的每行前几个字符(1-4 表示取前四个字符)


elasticdump  --input=http://127.0.0.1:9200/potatoes --output=http://192.168.10.18:9200/potatoes --all=true --limit=500000 


while read LINE; do echo $LINE|cut -d ',' -f 1,2 >>./ashleymadisondump-data/oneperday_am_am_member_1.txt; done < ./ashleymadisondump-data/oneperday_am_am_member.txt; 取文件第几列 (1,2)


for i in `ls`; do `cat  $i >> ../all_files.csv` echo $i; done; 合并文件夹下所有文件


number=0;for i in `ls`;do line=`wc -l $i`;line=${line% *}; let number+=$(($line)); echo $number;done; echo $number; 获取文件夹下所有文件行数

sed '1i\email,password,text' 6.fliter 文件插入首行

sed -n ‘2p’ file 输出文件第二行

perl -pi -e 's/\r//g' _.1400W-1600W.csv.flitere 删除\r

awk '{a[$1]++} END {for(i in a){print i,a[i] | "sort -r -k 2"}}' position.csv > r.txt  计算文件中 文字出现的次数
```

