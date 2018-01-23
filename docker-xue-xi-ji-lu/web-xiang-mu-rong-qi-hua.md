## web 项目容器化

- 前言

> 其实我是一直有这样的需求就是本地的开发环境与生产环境保持高度一致，但是保持高度一致是件很困难的事情，我们正常的开发环境都是Windows，但是生产环境常常回事linux，很多时候我们本地好好的代码到服务器上就跑不起来呢？想过打包本地的开发环境到生产环境，我想这么迫切的需求市面上一定有对应的解决方案，果然我找到了docker。

- 准备

我这里以javaweb项目为例，一个javaweb项目需要哪些配置呢，大致需要以下的配置：服务器环境（linux）、JDK、mysql，这里我是以springboot项目为例的，我们继续分解一下，我们需要什么我们需要一个linux环境，该环境装有1.8版本的JDK,装有5.7版本的mysql，容器在第一次启动的时候将初始化数据库，在数据库初始化完成后，将启动你的javaweb项目。

下面是最根本的，你需要在你的本地安装一下docker软件，不然怎么制作docker镜像呢！

下面是需要准备的文件，文件的目录如下：

![](/img/docker/javadocker.png)

发布一个打包为jar的javaweb 项目需要准备额就是以上的原料。

- 创建发布镜像

在上图中我们可以看到一个dockerfile文件，该文件是创建docker镜像的基础，dockerfile可以说是一个配置文件，改配置文件中说明了这个镜像的各个配置项我们可以大致的参观下：

````dockerfile
# 版本信息
FROM mysql:5.7
MAINTAINER daisy "jingchenxu2015@gmail.com"

# 创建与daisy相关的文件夹
RUN mkdir /daisy

# 设置mysql免密码登录
ENV MYSQL-ALLOW_EMPTY_PASSWORD yes

# 初始化mysql 参考http://www.jb51.net/article/115422.htm
COPY setup.sh /daisy/setup.sh
COPY daisy.sh /daisy/daisy.sh
COPY daisy.sql /daisy/daisy.sql
COPY privileges.sql /daisy/privileges.sql
#CMD ["sh", "/daisy/setup.sh"]

# 安装jdk环境
ADD jdk-8u162-linux-x64.tar.gz /daisy/
ENV JAVA_HOME=/daisy/jdk1.8.0_162
ENV PATH $PATH:$JAVA_HOME/bin

# 对外暴露端口号
EXPOSE 4000

# 启动daisy
ADD /daisy-0.0.1-SNAPSHOT.jar //
#ENTRYPOINT ["java", "-jar", "/daisy-0.0.1-SNAPSHOT.jar"]
ENTRYPOINT ["sh", "/daisy/setup.sh"]
````