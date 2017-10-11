## ubuntu 开发环境配置

* 下载安装

本次在虚拟机环境下安装的ubuntu17.04版本，下载地址为[ubuntu17.4](http://cn.ubuntu.com/download/),使用360浏览器的话还会下载的快一点，一般只在下载的时候用360浏览器。

虚拟机可以安装在移动硬盘当中，可以整体的复制移动虚拟机文件，存放到指定的移动硬盘的文件夹中，通过vmware 中的 文件-&gt;打开 找到对应的文件，即可重新挂在虚拟机。

* 安装JDK

下载Linux版本的JDK,解压好，然后放到Linux上，主要的工作是配置下bashrc,通过`vi ~/.bashrc`,打开rc文件，在文件的最底部添加以下内容：

```shell
export JAVA_HOME=/xxx/jdk
export JRE_HOME=$JAVA_HOME}/jre
export CLASSPATH=.:{JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

* 常用软件

tree、glances、[yu-writer](https://ivarptr.github.io/yu-writer.site/)、mycli

