## docker 安装MySQL

- 获取MySQL的最新docker镜像

````shell
docker pull mysql/mysql-server:latest
````

- 获取完成后我们可以看看MySQL的docker镜像在不在

````shell
docker images
````

- 我们需要创建一个docker容器用于承载MySQL镜像，具体的名利如下：

````shell
docker run --name mysql -p 3308:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql/mysql-server:latest
````

我们分析一下这段命令，我们创建的MySQL的容器名为mysql,在容器的内部MySQL自动占用的端口为3306，但是在主机中我们是无法直接访问容器内部的端口的，这时候就需要我们做一个映射，将容器内的端口映射到容器外，上面的命令是将容器内部的3306端口映射到容器外部的3308端口。

- 查看正在运行的容器

````shell
docker ps -s
````

- 接下来我们启动容器

````shell
docker start <container id>
````

- 进入容器内部

````shell
docker exec -it <container id> bash
````

- 进入MySQL数据库

````shell
mysql -uroot -p -h localhost
````