## MySQL 笔记

_连接数据库_

```sql
mysql -uroot -p
```

_退出数据库_

```sql
//注意要添加分号
exit;
```

_查看数据库_

```sql
SHOW DATABASES;
```

_创建数据库_

```sql
CREATE DATABASE foxebook;
```

_进入数据库_

```sql
USE foxebook
```

_查看数据库中的表_

```sql
SHOW TABLES
```

_批量修改存储过程创建者_

```sql
update mysql.proc set DEFINER='usename@%' WHERE NAME='proc_name@%' AND db='mydb';
```

## win10 下安装解压版mysql

[安装教程](http://www.cnblogs.com/tongy0/p/6739188.html)

> 需要注意的是原来的修改root密码的命令，目前mysql-5.7.18不支持在命令行中修改密码，对于window端可以通过my.ini文件进行修改。

_启动命令_

```sql
net start mysql
```

_关闭命令_

```sql
net stop mysql
```

_创建用户_

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

_数据库授权_

```sql
GRANT ALL ON *.* TO 'pig'@'%';
```

> 如果创建不了试试`FLUSH PRIVILEGES`

[mysql-创建用户并授权，设置允许远程连接](http://www.cnblogs.com/gpdm/p/6492449.html)

## linux 下安装多个版本MySQL

[安装教程](http://www.cnblogs.com/notester/p/5130713.html)

> 安卓完成后的数据库的维护是一个问题





