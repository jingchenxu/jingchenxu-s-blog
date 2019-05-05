# tomcat 使用指定版本JDK运行

* JDK && JRE

JDK:Java开发工具;JRE:Java运行时环境;

JRE 是 JDK的子集，JDK主要是多了一些开发工具，所以在生产环境安装完JDK就可以了。

* tomcat指定JDK版本

在生产环境的服务器中，可能一个服务器需要部署多个系统，如果都是自己公司的项目，还好说，大家都用的是同一个版本的JDK，那么每个tomcat使用全局的JDK是没有问题的，但是问题就在于，如果是客户提供的服务器，在服务器上已经跑了一个上古时期的项目，用的是JDK6。

这个时候就需要指定自己的tomcat使用的JDK 的版本，如果你不指定则会使用当前系统环境的全局JDK版本，可以查看tomcat/bin/setclasspath.sh文件，可以看到tomcat的JDK取自当前系统环境。

如果你需要指定tomcat运行是使用的JDK可在catalina.sh中指定JDK，只需要添加一下代码：

```bash
export JAVA_HOME=/xxxxx/java/jdk1.8.0_131
export JRE_HOME=/xxxxx/java/jdk1.8.8_131/jre
```

上面的代码是我在Linux环境下的配置，如果你是winserver，只需要修改对应的bat文件即可，可以观察到上面修改的配置为JDK的配置地址，所以需要先安装JDK。

