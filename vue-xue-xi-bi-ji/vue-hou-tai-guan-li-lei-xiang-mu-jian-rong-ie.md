# vue 后台管理类项目兼容IE

> 最近项目进入到了第三方集成的环节，对方非要用IE,咋办？老板让我将后台管理系统的框架兼容下IE.

## 目前后台管理系统前端搭建方式

目前系统是用vue-cli@2.0生成的，UI框架使用的是iview,ajax请求使用的是axois,此外就没有什么特殊的npm包了。

## 需要解决的兼容问题

1. 在IE下由于没有promise，所以axios不能用了；
2. 在涉及到flex,fixed,absolute定位时，IE浏览器的显示效果有较大的区别；
3. excel表单导出异常；
4. 部分使用的npm包中的代码未经编译会有一些ES6的语法；
4. vue-router路由失效。

## 如何解决这些问题

1. 解决第一个问题需要在项目中引入babel-polyfill， 我的处理方式时在build->webpack.base.config.js文件中的添加一下的配置：




