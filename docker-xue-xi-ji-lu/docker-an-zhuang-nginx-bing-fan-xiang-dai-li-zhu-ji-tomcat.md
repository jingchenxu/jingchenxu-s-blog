# docker 安装nginx并反向代理主机tomcat

> 最近要部署一个电商的服务器环境，主要涉及到的tomcat、nginx、redis的部署，我个人不是喜欢什么都往服务器上装，有些软件难以卸载干净，安装或是升级都会或多或少的残留一些东西，时间长了服务器会被一些不知道的东西占用资源。

## 服务器部署示意图


## docker 安装nginx


## docker 安装redis

docker 安装redis的方式比较简单，执行以下的命令就可以安装了：

````bash
docker run -d -p 6379:6379 --name myredis registry.docker-cn.com/library/redis
````

通过以上的推测，我们可以知道，服务器的主机在docker的虚拟网络中的ip为 ````172.17.0.3```` 所以redis的访问地址为： ````172.17.0.1:6379```` 


## tomcat 配置负载均衡



