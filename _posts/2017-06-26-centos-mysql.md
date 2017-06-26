---
layout: post
title: Centos上node连接mysql
---
如果centos上无法使用ifconfig，需要安装`net-tools`

```
yum install net-tools
```

安装ssh

```
sudo yum install openssh-server
```

安装mysql

```
yum install mysql
yum install mysql-server
yum install mysql-devel
```

修改mysql配置文件

```
vi /etc/my.cnf
```

修改用户密码

```
mysqladmin -u root -p password
```

安装node

```
sudo yum install nodejs
```

[mysql模块](https://github.com/mysqljs/mysql)

```
npm install mysql
```

测试index.js

```
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : '127.0.0.1',
  user     : 'root',
  password : '123456',
  database : 'mysql'
});

connection.query('select Password from user WHERE User = "root"', function (error, results, fields) {
  if (error) throw error;
  // connected!
	console.log(results,fields);
});
```
