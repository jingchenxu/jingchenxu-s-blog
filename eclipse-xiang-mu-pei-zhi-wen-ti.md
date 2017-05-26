## eclipse 项目配置问题

> 前端狗的一个package.json 一个 npn i,好前端框架搭建完成，对于一个cli生成的项目，接下来你就可以安安心心的写你的业务代码了，但对于一个java项目，可能是我的运气不好，每次从git上拉下来，本地总是跑不起来，总要向后端老手们请教，问题是我看他们调试几乎也是靠猜，而我本机出现的问题他们都没有遇到过，对于前端的项目编译（打包）我对其流程相对比较清除，哪里出现了问题都能很快的定位，问题是eclipse中的项目不好定位。

一个java项目的目录结构如下：

![此处显示的是如何设置的图片](/img/eclipse/menu.png)

注意上图是通过Package Explorer项目浏览窗口查看的，个人感觉这里是通过代码的逻辑进行展示的目录，Navigator 是项目的物理文件浏览窗口。

首先1、2是项目编译的源代码文件的入口，我们写的业务代码就是存放在这里的，那么是如何设置项目的编译入口文件夹的呢？点击你的项目右击：projectName->Properties->Resource->Java Build Path->Source,如下图：

![此处显示的是如何设置的图片](/img/eclipse/page1.png)

其实这边的入口文件路径应该实在项目初始化的时候就有的，被保存在.settings 文件夹中，这是个很让人蛋疼的文件夹，在这个文件夹中，会记录很多的信息，如项目编译的jdk版本，编译是入口文件的路径，输出的文件，想想与前端工程化的项目也很一致啊！

但是有一个问题就是可能各个开发者的开发环境是不一致的，这就导致了一个项目拖到另一台电脑上就跑不起来了，所以写readme 啊！

1、2两项能否正确配置，还需要查看.settings文件夹下的 org.eclipse.wst.common.component 文件，这个文件中记录了额以上信息，如果记录的信息与你的开发环境不符，则需进行修改。

```xml
<?xml version="1.0" encoding="UTF-8"?><project-modules id="moduleCoreId" project-version="1.5.0">
    <wb-module deploy-name="nestle">
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/resources"/>
        <wb-resource deploy-path="/" source-path="/webapp" tag="defaultRootSource"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/java"/>
        <property name="context-root" value="nestle"/>
        <property name="java-output-path" value="/nestle/build/classes"/>
    </wb-module>
</project-modules>
```

促使我写这篇文章的动力就是，项目中有人修改了jar包放置的目录，但是在这个文件中还是指向原来的路径，他就是<wb-resource deploy-path="/" source-path="/webapp" tag="defaultRootSource"/>
文件中写的是<wb-resource deploy-path="/" source-path="/WebContent" tag="defaultRootSource"/>，实际路径明明变了，这导致了我一致编译不了，1、2在项目目录中一直不显示，同时你设置的编译的入口文件夹的路径也会影响你在Package Explorer 看到的java包名，如果你的这些配置正确的化，你查看项目应该与下图一致：

![此处显示的是如何设置的图片](/img/eclipse/page2.png)

并且3、4、5显示正常。

对于在eclipse中启动tomcat，但是项目没有发布到tomcat时，首先先排查项目有没有正确的编译，主要是看你的类似build target之类设定的编译文件输出目录中又没有.class文件，如果没有，这说明你的编译配置有问题，主要看编译的输入输出路径设置有没有问题，包的加载有没有问题，如果不是编译的问题，正常编译除了.class文件，则查看是不是tomcat的发布存在问题，主要的问题集中在tomcat插件的配置，和tomcat中server.xml的配置可能出现问题。



