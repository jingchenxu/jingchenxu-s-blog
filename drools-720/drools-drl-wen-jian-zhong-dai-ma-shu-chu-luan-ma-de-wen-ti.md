## drools drl 文件中输出中文乱码问题

有时候我们会在规则文件中添加一些输出，为了查看规则文件中写的java代码是否按照预想进行，但是在eclipse和intelij中遇到了相同的问题，那就是规则文件中java代码中的````java System.out.println("你好") ```` 输出的为乱码，相同的问题。

之前项目组其他成员在使用eclipse时的时候，发现了这个问题，出现中文乱码，我们的第一反应多半是文件的格式可能不对，所以查找了项目的文件格式之类的配置，但是很遗憾不是文件格式的问题，最后再同事的提醒下，可能是工作空间的问题，于是查看eclipse的工作空间的编码格式设置：

![eclipse](/img/Drools/drools10.png)

所以在我切换使用intelij的时候，出现同样的问题，我觉的也应该是工作空间的问题，但是intelij的配置可能有点区别，首先你需要修改intelij安装目录下的jvm的配置文件，具体修改内容为：

![eclipse](/img/Drools/drools11.png)

在安装目录下找到这2个文件，在文件的最后一行添加一下内容：

-Dfile.encoding=UTF-8

此外还需要在tomcat中配置一下，具体配置可见下图：

![eclipse](/img/Drools/drools12.png)



