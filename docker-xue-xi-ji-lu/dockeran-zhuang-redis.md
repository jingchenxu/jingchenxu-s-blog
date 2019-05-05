# docker安装redis

最近用了docker以后发现再也不用担心把服务器环境搞坏了，每个应用都运行在容器之内，所以打算redis也使用docker安装一下，如果是简单的安装的话，是可以参考MySQL是怎么安装的，但是redis的安装后好需要进行一些配置，这个时候就出现问题了，当我通过`docker attach <container_id>`,发现无法进入到容器内部，好吧！我们就暂且不管了，我们还可以使用这个命令`docker exec -it <container_id> bash`,在进入到容器内部后，发现不行啊，找不到redis的配置文件在哪里，后来进过查阅，目前有可以有2中安装思路。

* 思路1

下载镜像

```text
docker pull redis
```

上面这一步是没什么特别的，主要是在创建容器的时候需要注意，需要配置容器内部的配置文件，我们先看一下容器的创建参数设置,我们首先在宿主机器上创建一个文件夹作为容器的外部配置文件的地址：

```text
mkdir jingchenxu
cd jingchenxu
mkdir redis
cd redis
// 下载redis安装包，用于获取redis.conf配置文件
wget http://download.redis.io/releases/redis-stable.tar.gz
// 解压安装包
tar xzf redis-stable.tar.gz
// 创建容器
docker run -p 6379:6379 --name redis -v $PWD/redis.conf:/jingchenxu/redis/redis-stable/redis.conf -v$PWD/data:/jingchenxu/redis -d redis redis-server /jingchenxu/redis/redis-stable/redis.conf --appendonly yes
// 启动容器
docker start redis
```

以上容器中的redis则会读取，/jingchenxu/redis/redis-stable/文件夹下的配置文件，修改配置文件后，重启下容器即可生效。

查看redis所在容器的内部IP地址

```text
// 查看容器IP地址
docker inspect <container_name> | grep IP
```

在宿主机器上连接redis

```text
docker run -it redis redis-cli -h 127.17.0.3
```

* 思路2

该方案自己还没有实践过，但是大致的思路是在一个使用Linux镜像的容器中安装redis，开启对应的端口映射即可。

