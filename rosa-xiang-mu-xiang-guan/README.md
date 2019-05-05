# Rosa 项目相关

> 尝试了WPF,javaFX,pythonwx，最后发现可能还是electron比较适合我，其他的语言对我来说基础比较的薄弱，画一个组件还要找半天的的技术文档，最后看了下可能还是electron这个技术比较合适，有段时间没有使用react了，所以打算瞧瞧react有什么变化。

## 使用create-react-app创建项目

首先你需要全局安装create-react-app包，这个包做了很多的事情，让我们从搭建前端开发环境的苦逼操作中解放出来了，但是你最好了解其是怎么工作的。

再创建完react项目之后，我们需要向该项目中添加electron相关的依赖，并新增Electron主线程代码，[electron项目创建教程](https://electronjs.org/docs/tutorial/application-architecture)，再项目的根目录下添加main.js作为electron项目主线程的入口代码，并按照文档创建主线程的启动命令。

## 配置注意项

