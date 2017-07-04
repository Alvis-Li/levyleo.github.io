---
layout: post
title: mysql判断db、table是否存在
---

判断table是否存在

```
show tables like

```

判断db和table是否存在

```
SELECT DISTINCT t.table_name, n.SCHEMA_NAME FROM information_schema.TABLES t, information_schema.SCHEMATA n WHERE t.table_name = 'tableName' AND n.SCHEMA_NAME = 'databaseName';  

```