## wordpress 安装

- 安装apache2

````bash
sudo apt-get install apache2  
````

apache2安装后默认占据的端口为80端口，因为我安装了nginx，所以这里需要修改apache2的默认端口，通过以上命令安装的apache2,需要到/etc/apache/目录下修改ports.conf文件的端口配置，我这修改为了9000端口，修改完成后你需要重启，在/etc/init.d目录下执行

````bash
./apache2 restart
````

命令是apache端口修改生效

- 安装php

你需要先安装php，然后再安装apache2 php，安装命令如下：

````bash
sudo apt-get install php7.0
sudo apt-get install libapache2-mod-php7.0
````

安装完成后我们需要测试一下php是否安装成功，我们需要在apache2的默认跟目下创建一个php文件用于测试php是否安装成功，apache2的根目录位于/var/www/html下，在该目录下创建info.php文件，文件内容为：

````php
<?phph
phpinfo
?>
````

访问yourIP:9000/info.php 是否显示php版本信息

- mysql的安装请参考本博客的其他文章

- wordpress安装

以上安装完成后，需要在apache2跟目下部署wordpress源码即可。