## MySQL 笔记

*连接数据库*

```sql
mysql -uroot -p
```

*退出数据库*

```sql
//注意要添加分号
exit;
```

*查看数据库*

```sql
SHOW DATABASES;
```

*创建数据库*

```sql
CREATE DATABASE foxebook;
```

*进入数据库*

```sql
USE foxebook
```

*查看数据库中的表*

```sql
SHOW TABLES
```

## win10 下安装解压版mysql

[安装教程](http://www.cnblogs.com/tongy0/p/6739188.html)

>需要注意的是原来的修改root密码的命令，目前mysql-5.7.18不支持在命令行中修改密码，对于window端可以通过my.ini文件进行修改。

*启动命令*

```sql
net start mysql
```

*关闭命令*

```sql
net stop mysql
```

*创建用户*

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password'; 
```

>如果创建不了试试```FLUSH PRIVILEGES```










