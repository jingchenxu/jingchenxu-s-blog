## javascript 构造函数与原型

- 构造函数与普通函数

构造函数与普通函数都是函数，我们可以看下面这段不是很规范的代码，不规范的地方在于，构造函数的的首字母没有大写，这是为了让我们更清楚的看到构造函数与普通函数没有什么区别，或者网上认为首字母大写的函数是构造函数是不正确的，构造函数应该是这样的函数 ```` new Demo() ````，构造函数基于普通函数我们可以看一下下面的代码：

````javascript
            function common() {
                console.log('this is common');
                console.log(this);
                return true;
            }

            var common1 = new common();
            var common2 = common();

            console.dir(common1);
            console.dir(common2);
````

![此处显示的是如何设置的图片](/img/javascript/result1.png)

不知道可不可以这么理解，通过关键字new 实现了普通函数向构造函数的转变，new关键字改变了函数内部 this 的指向，改变了函数最后的返回值。

| 特性          | 构造函数       | 普通函数 |
| ------------- |:-------------:| -----:|
| this指向      | 构造函数中的this指向的是当前构造函数 | 指向window |
| 函数返回值      | 返回的是构造函数实例      |   返回函数返回值 |
| 函数命名方式 | 根据默认约定首字母应该大写     |    采用通畅的小写或是驼峰命名 |

- 构造函数与对象

构造的主要作用是创建对象，但是构造函数本身也是一个对象，因为在javascript中函数也是对象，




- 相关文档

[深入理解javascript构造函数和原型对象](http://www.jb51.net/article/55539.htm#card_1508137382789_9336)