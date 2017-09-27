## docker 常见命令

1、列出所有的docker镜像

````shell
docker images
````

2、停止所有的container，这样才能够删除其中的images

````shell
docker stop $(docker ps -a -q)
````

3、删除所有的容器

````shell
docker rm $(docker ps -a -q)
````

4、删除指定容器

````shell
docker rm <container id>
````

5、删除指定的镜像

````shell
docker rmi <image id>
````

6、拉取官方镜像

````shell
docker pull mysql/mysql-server:latest
````

7、创建容器

````shell
docker run -dit busybox
````

8、查看所有的容器

````shell
docker ps -a
````

9、进入容器内部

````shell
docker attach <container id>
````

10、后台启动容器，并可用bash访问容器

````bash
docker run -d -it <image -id>
````

## docker 基础信息

1、查看docker版本

````bash
docker version
````

2、查看docker系统信息

````bash
docker info
````

