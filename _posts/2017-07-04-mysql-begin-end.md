---
layout: post
title: mysql使用BEGIN...END
---
通常begin-end用于定义一组语句块，在各大数据库中的客户端工具中可直接调用，但在mysql需要借助DELIMITER，也就是需要定义分界符。
例如：

```
delimiter $$
BEGIN $$
SELECT COUNT(*) from user $$
END $$
delimiter ;
```

