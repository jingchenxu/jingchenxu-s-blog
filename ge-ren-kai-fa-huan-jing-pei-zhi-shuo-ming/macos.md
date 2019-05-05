# macos

**开发语言**

* javascript
* java
* python

**文本编辑器**

* vscode
* sublime

**IDE**

* intellj
* eclipse

**JDK多版本配置**

```text
export JAVA_6_HOME=/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_73.jdk/Contents/Home
export JAVA_HOME=$JAVA_8_HOME

alias jdk8='export JAVA_HOME=$JAVA_8_HOME'

alias jdk7='export JAVA_HOME=$JAVA_7_HOME'

alias jdk6='export JAVA_HOME=$JAVA_6_HOME'
```

**将mysql命令添加到环境变量**

```text
sudo vim .bash_profile
export PATH="${PATH}:/usr/local/mysql/bin"
```

## mac软件

* [MWeb for Mac](http://zh.mweb.im/index.html)一款看起来很不错的markdown软件
* mycl 不错的命令端mysql命令补全工具

