## tomcat winserver 下部署开机自启动

- java环境准备

安装JDK,安装完成后设置系统环境变量，并进行测试是否成功，java安装后需要配置环境变量，主要有这样的几个变量需要配置

| 变量名          | 变量值       | 
| ------------- |:-------------:|
| JAVA_HOME      | C:\Program Files\Java\jdk1.8.0_144 | 
| PATH      | %JAVA_HOME%\bin  %JAVA_HOME%\jre\bin      | 
| CLASSPATH | .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;     |    

以上配置成功后，我们需要通过命令查看下java环境是否配置成功

![javaversion](/img/eclipse/javaversion.png)