# vue 渲染函数与函数组件

> 毕竟是从react迁移过来的，最近决定探索下vue的函数组件，于是发现了一些有趣的事情，由于一直使用模板的方式创建vue组件，导致我对vue产生了一些误解，认为vue必须要通过模板的方式创建组件，其实不然，我们还可以通过渲染函数和行数组件的方式创建vue组件，两者写法上比较类似，但是实际使用过程中存在较大不同。

## 渲染函数

通常情况下目前我们都会通过模板的方式创建组件，这种组件的文件一般都是以 .vue 结尾的，webpack通过匹配文件的后缀名，对vue文件通过vue-loader进行解析，就像是react以 .jsx 结尾的文件一样。

使用非模板的方式进行vue组件的编写，就是react所说的all in js，写惯了react的人会觉得这是不错的体验，在vue中还可以通过解析html字符串的方式实现创建组件，总体来说直接使用render函数开发组件的情况不是很多见，比较了解的是vuetify UI 组件库是使用渲染函数的方式创建组件的。
