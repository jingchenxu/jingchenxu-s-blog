# docker 常见命令

## docker 常见命令

1、列出所有的docker镜像

```text
docker images
```

2、停止所有的container，这样才能够删除其中的images

```text
docker stop $(docker ps -a -q)
```

3、删除所有的容器

```text
docker rm $(docker ps -a -q)
```

4、删除指定容器

```text
docker rm <container id>
```

5、删除指定的镜像

```text
docker rmi <image id>
```

6、拉取官方镜像

```text
docker pull mysql/mysql-server:latest
```

7、创建容器

```text
docker run -dit busybox
```

8、查看所有的容器

```text
docker ps -a
```

9、进入容器内部

```text
docker attach <container id>
```

```text
docker attach <id/container_name>
```

10、后台启动容器，并可用bash访问容器

```bash
docker run -d -it <image -id>
```

## docker 基础信息

1、查看docker版本

```bash
docker version
```

2、查看docker系统信息

```bash
docker info
```

## docker 日志相关

1、查看容器日志

```bash
docker logs <id/container_name>
```

2、实时查看容器日志

```bash
docker logs -f <id/container_name>
```

