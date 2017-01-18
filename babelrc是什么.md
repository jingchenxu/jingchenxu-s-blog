## .babelrc是什么

.babelrc一般是写在根目录下的，他的作用，我知道的就是设置一些转码的规则和插件，目前的情况下，如果你想使用ES6的语法，一般是需要安装插件的，在.babelrc中配置的插件是需要事先安装好的。

下面是一个例子：

````json
{
  "presets": ["es2015", "react"],
  "plugins": [
    "syntax-class-properties",
    //用于解析static、class等关键字
    "transform-class-properties"
  ]
}
````

其实为什么叫这个名字呢？我网上查了一下,rc结尾一般代表的是运行时自动加载的文件、配置等等，而.babelrc就是在babel进行转码时自动加载的配置文件，算是babel的配置文件吧！

相关的详细使用方法可以在这里查看：[babel官网](http://babeljs.cn/docs/usage/babelrc/)


