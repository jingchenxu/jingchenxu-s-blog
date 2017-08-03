## eclipse 使用不同版本JDK进行编译

在进行java项目开发的时候，最后编译生成war包，发布到生产环境，但是生产环境中的JDK的版本可能与你编译时使用的JDK的版本不一致，于是发布到生产环境的时候会产生各种各样的不兼容。

eclipse本身是可以设置不同的项目使用不同版本的JDK进行编译,具体设置如下：
右击需要编译的项目->Properties->Java Compiler->调节Compiler compliance level 选择对应的版本即可。

此外eclipse也可指定另外的jre版本。

