## springboot 项目注意事项

- 连接mysql数据库提示

提示信息如下：

>WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

在数据库配置连接的时候需要注意按照以下配置进行配置：````spring.datasource.url=jdbc:mysql://localhost:3306/Lottery?useSSL=false````

- 配置devtools

配置devtools时，可以开启系统监控，但是需要注意的是，开启监控后，需要制定哪些访问路径需要进行安全保护，如果没有设置路径的话，会默认的所有的路径都要进行安全保护，这里主要在properties文件中配置一下就可以了，具体的配置如下：

````properties
security.user.name=123
security.user.password=123
security.basic.enabled=true
security.basic.path=/man
management.security.roles=ADMIN
management.context-path=/manager
````

- 字体等静态资源访问被过滤问题

最常见的情况是在springboot项目中使用到字体图标时会出现这样的问题，字体文件在请求时会出现这样的问题：

>"Failed to decode downloaded font"和"OTS parsing error: Failed to convert WOFF 2.0 font to SFNT"

出现这样的问题，我们需要指定部分的静态资源不需要过滤，具体的设置可以在pom文件中添加如下的插件：

````xml
            <!--避免对一些静态资源进行filter-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <configuration>
                    <nonFilteredFileExtensions>
                        <nonFilteredFileExtension>ttf</nonFilteredFileExtension>
                        <nonFilteredFileExtension>woff</nonFilteredFileExtension>
                        <nonFilteredFileExtension>woff2</nonFilteredFileExtension>
                    </nonFilteredFileExtensions>
                </configuration>
            </plugin>
````

- 后台启动项目jar包

启动命令如下：

````shell
nohup java -Xms10m -Xmx40m -jar lottery.jar >lottery.log &
````

nohup 表示窗口关闭后系统继续运行
-Xms10m 表示springboot内置tomcat最小内存为10M
-Xmx40m 表示springboot内置tomcat最大内存为40M
>lottery.log 表示指定日志输出文件为lottery.log
& 表示后台运行