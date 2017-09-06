## spring 项目集成Drools6.5.0Final

- 开始

是的，公司的项目就是maven也没有用，就是一个eclipse生成的普通项目，然后倒入spring相关的jar包，自己搭建的一个框架。这样的一个项目因为项目的推进需要集成Drools。

- 为什么选择6.5.0Final版本

网络资源多？不是，而是不得不妥协的问题，目前项目的测试及生产环境的JDK版本都是1.7,而Drools7.0.0 7.2.0 折2个版本都是由1.8的JDK编译而成的，我们可以通过验证class的版本来确认，如果你强制用高版本的Drools,那么你的的项目会出现这样的问题，Unsupported major.minor version 52.0，这是低版本的jvm无法加载高版本的JDK编译的class文件导致的。

对于7.0.0Final版本的Drools中的class的版本进行校验，校验的方法如下，在class文件的根目录下查看class的版本：
![图片](/img/Drools/javaversion.png)


