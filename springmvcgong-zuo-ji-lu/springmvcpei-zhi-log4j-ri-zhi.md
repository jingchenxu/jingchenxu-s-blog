## springMVC配置log4j日志

> 开始写java有段时间了，主要使用的是spring + springMVC + Mybatis技术栈，如果是本地调试的话，在eclipse下还是很方便的，主要是服务器环境下出现的问题，很难排查，一般出了问题，自己很难定位到问题在哪里，这时候就需要使用到log4j,这个jar包来进行日志的记录。

* 安装日志jar包

在网上下载log4j-1.2.17.jar,下载完成后添加到classpath当中，接下来需要做的就是配置日志参数，配置文件名为log4j.properties,文件存放在classpath下，。

配置文件的配置如下：

```java
### 设置日志等级  ###

log4j.rootLogger = DEBUG,stdout

### 输出到控制台 ###

log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n

### sql ###

log4j.logger.java.sql.Connection=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG

### 设置 E###

log4j.rootLogger = debug,stdout,D,E

### 输出DEBUG 级别以上的日志=D://logs/error.log ###

log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File = /xxxxxx/xxxxxx/all/log.log
log4j.appender.D.DatePattern=yyyy-MM-dd-HH-mm'.log'
log4j.appender.D.Append = true
log4j.appender.D.Threshold = DEBUG
log4j.appender.D.layout = org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n 

### 输出ERROR 级别以上的日志到=E://logs/error.log ###

log4j.appender.E = org.apache.log4j.DailyRollingFileAppender
log4j.appender.E.File =/xxxxxxx/xxxxxxx/error/error.log
log4j.appender.E.DatePattern=yyyy-MM-dd-HH-mm'.log'
log4j.appender.E.Append = true
log4j.appender.E.Threshold = ERROR
log4j.appender.E.layout = org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n

### 用户登录日志 ###

log4j.logger.com.lvfulo.controller.custom.WXUserController= DEBUG, login
log4j.appender.login=org.apache.log4j.FileAppender
log4j.appender.login.File=/xxxxxx/xxxxxx/login/login.log
log4j.appender.login.DatePattern=yyyy-MM-dd-HH-mm'.log'
log4j.appender.login.layout=org.apache.log4j.PatternLayout
log4j.appender.login.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n
```

以上可以看到，可以将制定等级的日志输出到指定的文件夹的指定日志文件，还可以将特定class的日志输出到指定文件夹的文件当中，在示例中所有的日志文件都的名字当中都添加了时间。

* 注意事项

1、配置文件在eclipse当中通过默认设置显示时，中文会以转义字符串的方式显示，你需要在eclipse当中进行设置：Window-&gt;Preference-&gt;
![log4j](/img/springMVC/log4j.png)
按照此图进行配置则可正常显示中文。


2、考虑到很多人是在Windows下进行开发，但是系统是在Linux环境下部署的，如何设置日志文件的存放目录呢？如图所示，只需要按照示例中的地址写法即可，在Windows环境下，则会默认以代码所在盘为根目录。

3、需要注意在Linux环境下，你的日志文件写入的文件夹是否有权限写入。





