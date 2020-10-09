# Mysql 5.7（ubuntu） 在线安装及中文字符集设置

## 在线安装mysql

该教程为在ubuntu下安装mysql5.7

```
#更新apt-get索引
sudo apt-get update

#安装MySQL:
sudo apt-get install mysql-server

#查看MySQL版本: 
mysql -V

#进入MySQL: 
mysql -u root -p

#启动: 
sudo service mysql start

#重启:
sudo  service mysql restart 

#关闭: 
sudo service mysql stop
```

{% hint style="info" %}
 安装完成后root是没有密码的，可以直接登录到mysql当中
{% endhint %}

设置root远程登录密码

```bash
update user set authentication_string=password('新密码') where user='root'
```

### 设置mysql允许远程登录、中文字符集、忽略表名大小写

mysql 配置文件目录为 /etc/mysql/mysql.conf.d/mysqld.cnf 通过vi编辑该文件，具体需要修改以下内容：

```bash
[mysqld]
lower_case_table_names=1
character_set_server=utf8
bind-address=0.0.0.0
```

