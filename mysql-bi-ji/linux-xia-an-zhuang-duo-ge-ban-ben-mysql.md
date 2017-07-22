## Linux 下安装多个版本MySQL

安装教程可参考这篇文章：[安装教程](http://www.zuimoban.com/jiaocheng/mysql/3679.html)

- 安装后从shell登录

```shell
/usr/local/mysql5.6/bin/mysqladmin -u root -p 3316 -s /data/mysql5.6/mysql.sock
```

- 重启MySQL

```shell
ps -ef|grep mysql
```
找到对应的进程后通过kill命令关闭MySQL

```shell
/usr/local/mysql5.6/bin/mysqld_safe --defaults-file=./my.cnf --user=mysql &
```



