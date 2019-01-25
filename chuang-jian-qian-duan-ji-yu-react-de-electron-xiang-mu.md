# 创建前端基于react的electron项目

> 尝试了较多的桌面应用开发方式后，最后发现可能electron是最好的选择，我就是想做一个根据数据库表结构生成代码的工具，这样的应用就是将一个web应用转化为一个桌面应用，之前试过WPF,javaFX,pythonwx,他们都不错，但是有2点不好看，我不熟,想要画出好看的GUI不容易。

## 创建一个create-react-app项目

create-react-app是个类似vue-cli的项目创建工具，比2年前好用多了，以后升级也方便多了，再也没有一步步从webpack搭建一个工程化项目的痛苦，但却有点怀念。

我们创建好一个create-react-app项目后，开始通过yarn添加electron依赖，添加完成后，设置后端进程的代码入口，这里设置为main.js,通过create-react-app创建的项目的前端入口文件应该是index.js,electron项目中一般会有2个进程，这2个进程通过rpc进行通讯。

