## VPS 折腾手册

> 每次换云服务器都很蛋疼一些软件迁移什么的比较麻烦，所以想着记录下来，方便每次迁移，为啥迁移呢！因为每次买的时候都是活动价，结果续费的时候贵的要命，教程基本是基于ubuntu的，为啥啊！因为我比较熟悉。

- 设置su账号密码

````bash
sudo passwd
````

- 更新Linux

更新源：
````bash
sudo apt-get update
````

更新Linux
````bash
sudo apt-get upgrade
````
注意更新Linux可以自动更新内核，下一步的升级内核可以不用操作了。

- 升级Linux内核

通过 :
````bash
uname -sr
````
 检查当前云服务器的内核版本，可在http://kernel.ubuntu.com/~kernel-ppa/mainline/ 该地址找到最新的的Linux内核，并下载，下载完成后上传到服务器指定目录下，并在该目录下运行:
````bash
sudo dpkg -i *.deb
````

- 开启BBR

修改系统变量

````bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.confecho "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
````

保存生效

````bash
sysctl -p
````

检测是否生效

````bash
lsmod | grep bbr
````


