## react-native 与 react 的区别

经过一段时间的了解，总算是弄清楚了react-native的基本知识，关于react-native 的开发环境的搭建过程就暂时先不说了。

### 路由

在react中的路由可以采用react-router这个包来实现，根据文档配置好，就可以使用其的诸如跳转等等功能。

但是在react-native当中，其依赖于react-native中的Navigator组件来实现，该组件中自带部分的跳转动画，按照指定的配置即可。

对比一下：react中的路由其实是由a标签来实现的，但是在react-native中实现跳转则是与iOS原生实现方法类似，通过栈的进出来实现的。

> TODO:添加与此相关的代码

### 页面渲染

