## 浏览器如何兼容ES6

- 逆工程化

首先个人对于工程化是不排斥的，个人还非常喜欢工程化，喜欢babel，但是并不是所有的项目都需要工程化，有些时候我们只要写12个页面，纯手写js么有不想，想用react、vue呢难道又要初始化个项目，我们想要的就是扔到服务器就能跑的代码，所以我们在浏览器中使用babel吧！其实我们的chrome基本上完全支持ES6了，但是考虑到广大的浏览器，我们还是使用一下babel吧！

- 方法一

在浏览器中引入babeljs,通过制定javascript标签引入是设置的标签属性：

````html
<script src="https://cdn.bootcss.com/babel-standalone/7.0.0-beta.2/babel.js"></script>
<script src="./config/babelTest.js" type="text/babel"></script>
````

