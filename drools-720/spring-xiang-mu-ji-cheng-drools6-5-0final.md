## spring 项目集成Drools6.5.0Final

- 开始

是的，公司的项目就是maven也没有用，就是一个eclipse生成的普通项目，然后倒入spring相关的jar包，自己搭建的一个框架。这样的一个项目因为项目的推进需要集成Drools。

- 为什么选择6.5.0Final版本

网络资源多？不是，而是不得不妥协的问题，目前项目的测试及生产环境的JDK版本都是1.7,而Drools7.0.0 7.2.0 折2个版本都是由1.8的JDK编译而成的，我们可以通过验证class的版本来确认，如果你强制用高版本的Drools,那么你的的项目会出现这样的问题，Unsupported major.minor version 52.0，这是低版本的jvm无法加载高版本的JDK编译的class文件导致的。

对于7.0.0Final版本的Drools中的class的版本进行校验，校验的方法如下，在class文件的根目录下查看class的版本：
![图片](/img/Drools/javaversion.png)
我们主要查看的信息是 major version:52 52标识的是由JDK1.8编译生成的class。

- 如何集成

其实最简单的方法可以是将Drools相关放到WEB-INF的lib文件夹当中，当然也是可以添加一个jar library,但是需要注意的是，在打包为war包是，需要将该library添加到最后发布的war包当中，具体设置如下：
![图片](/img/Drools/class.png)
如图图中的JUnit 4library是会被添加到最终发布的war包的WEB-INF的lib文件夹中的。
当然你也可以将drools相关的jar包都存放到WEB-INF的lib文件夹当中，其实使用drools就是引入drools相关的jar包然后进行调用，这只是最简单的用法。



- 出现中文乱码怎么办

Drools运行时的编码使用的是系统的默认编码，所以需要注意你的系统的默认编码是什么，此外还需要注意你的文件编码是什么，一般最好设置你所有的编码格式为UTF-8，网上搜一下，Drools中文乱码的原因，我找了一下大概还要一种可能没有涉及到，那就是eclipse的工作空间的编码格式，如下图：
![图片](/img/Drools/Drools6.png)

- 备注

Drools各版本下载地址：![Drools下载地址](http://download.jboss.org/drools/release/)






