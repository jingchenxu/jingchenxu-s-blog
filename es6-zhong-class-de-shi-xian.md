## ES6 中class的实现

### 构造函数

无论是ES5还是ES6，javascript中继承的实现都离不开构造函数，构造函数可以看做是一个模板，根据这个模板，通过传入不同的参数，可以创造出不同的对象。

构造函数的大致结构图：
![](/img/class/construct.png)

#### 关于图中出现的几个东西

对象1（图中对象右上角所标注的序号）的prototype：构造函数一个属性，他是一个指针，他指向的是对象2（原型对象）；

对象2的constructor: 他是对象2的一个属性，他是一个指针，指向构造函数

对象4、5、6的__proto__: 他是对象4、5、6的一个属性，指向构造函数对应的原型对象。

测试用例：

```javascript
<html>
    <head>
        <title>js的原型对象和constructor研究</title>    
    <head/>
    <body>
    
        <script >
            /**
            *javascript原型对象是一种特殊的实例，它提供一种在所有的实例中共享状态的机制
            *当访问对象成员时,先在当前对象中查找,,如果没有找到,则到该对象的原型对象中进行查找,,,一直到Object
            */
        
        //为Object的原型对象添加属性
        Object.prototype.sex = "male";
        
        function Base(){
            this.name = "eric";
        }
        
        Base.prototype = {
            constructor:Base,    //把原型对象的constructor指向Base
            name:"prototype..",
            greet: function(){ alert("hello...i'm  " + this.name + "and i m " + this.sex)}    //此处的sex继承自Object
        }
            
        var a = Base.prototype;
        a.greet();    //prototype
        var obj = new a.constructor;    //用原型对象的构造方法创建对象
        alert(obj.name);    //eric
        var b = new Base();
        b.greet();    // eric    
    
        </script>
</body>
</html>
```

### chrome61支持class啦

时光荏苒，岁月如梭，最近发现chrome61已经开始支持import了，可喜可贺，基本上ES6的属性都可以在chrome61上开跑了。



### 参考文档

【1】[JS的构造函数](http://www.cnblogs.com/jikey/archive/2011/05/13/2045005.html)
【2】[JavaScript构造函数详解](http://www.jb51.net/article/77039.htm)
【3】[Class基本语法](http://es6.ruanyifeng.com/#docs/class)
【4】[]()
【5】[面向对象中构造函数,原型对象和实例的关系图](http://www.cnblogs.com/yuluo2016/p/5894698.html)<br/>
【】[小记ES5和ES6的类](https://zhuanlan.zhihu.com/p/26070788)