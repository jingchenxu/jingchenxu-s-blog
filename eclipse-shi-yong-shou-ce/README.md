# eclipse 使用手册

最近在虚拟机的Linux环境中遇到了一些问题，主要是tomcat无法启动的问题。

* 问题1：没有此命令

```bash
-bash: ./startup.sh:权限不够
```

怎么办没权限给权限：

```bash
sudo chmod -R 777 startup.sh
```

需要注意的是一个shell脚本可能会调用到其他的shell脚本，这个时候其他的shell脚本同样的也会需要相应的权限才能执行，我们需要做的就是将所有的shell脚本的权限修改一下，这只是临时的解决方案。

最近遇到一个比较蛋疼的问题，tomcat启动时会出现如下的提示： Neither the JAVA\_HOME nor the JRE\_HOME environment variable is defined At least one of these environment variable is needed to run this program

看了，这边的代码是出现在，setclasspath.sh这个shell脚本当中，这个脚本的作用是为了获取当前环境的 JAVA\_HOME ,但是不幸的是，在shell脚本中并没有找到 JAVA\_HOME ，为此我特地写了一个 test.shell,内容为echo $JAVA\_HOME, 如果直接在命令行中输入此命令，是可以找到 JAVA\_HOME 的，但是在shell脚本中就不可以，也就是我的 java 的环境变量应该是没有问题的，问题可能出在shell脚本的额执行权限上面，于是我将用户切换为root用户，启动tomcat就没有问题了。

