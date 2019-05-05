# Linux 下安装多个版本MySQL

安装教程可参考这篇文章：[安装教程](http://www.zuimoban.com/jiaocheng/mysql/3679.html)

* 安装后从shell登录

```text
/usr/local/mysql5.6/bin/mysqladmin -u root -p 3316 -s /data/mysql5.6/mysql.sock
```

* 重启MySQL

```text
ps -ef|grep mysql
```

找到对应的进程后通过kill命令关闭MySQL

```text
/usr/local/mysql5.6/bin/mysqld_safe --defaults-file=./my.cnf --user=mysql &
```

> 注意上面的shell脚本中./my.cnf 是这么写的，也就意味着你必须在my.cnf文件所在的文件夹的目录下执行此命令，当然你可以在这里写绝对路径， it's up to you.

