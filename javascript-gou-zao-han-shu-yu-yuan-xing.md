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

````javascript
            // Demo构造函数的的父构造函数微Function（）
            // 所以对比通过Object（）构造函数创建对象，本质上通过自定义构造函数创建对象与Object()构造函数创建对象本质上是一样的
            // 通过观察 constructor 对应的父级构造函数可以看到其是由谁继承而来的
            // 构造函数本身也是对象 也是有__proto__的，构造函数对象的原型为


            log.log_title('构造函数与对象');

            // 创建构造函数
            var Demo = function(a,b){
                this.a = a;
                this.b = b;
            }

            // 指定原型
            Demo.prototype = (function(){
                var add = function(){
                    return this.a + this.b;
                };

                var sub = function(){
                    return this.a - this.b;
                };
                return {
                    add: add,
                    sub: sub
                }
            }());

            console.dir(Demo);

            var demo1 = new Demo(1,2);
            // 对象中只有__proto__ 指向的是该对象的构造函数的原型 prototype
            console.dir(demo1);
            // 使用字面量创建对象 本质上还是通过Object()构造函数创建对象
            var demo2 = {
                a: 1,
                b: 2
            };
            console.dir(demo2);
````

- \_\_proto__与prototype

当我们通过console.dir(object)时，我们可以在对象的属性里看到\_\_proto\_\_属性和prototype属性，当然在很多的时候你是看不到prototype属性的，但是 \_\_proto\_\_属性是肯定能看到的，而\_\_proto\_\_也就构成了我们所说的原型链，当我们通过this调用相关的属性的时候，我们会先到对象自带的属性当中查找，挡在对象的自身属性中查找不到的时候，就会到该对象的\_\_proto\_\_中进行查找，如果在\_\_proto\_\_中没有找到的话，会继续从\_\_proto\_\_的\_\_proto\_\_中去找，最后会指向到Object原型，这个时候的\_\_proto\_\_就没有了，如果到这个时候还没有找到，则会返回undefined。

上面顺带的介绍了原型链，2个东西我们最先想到的就是比较\_\_proto\_\_与prototype的区别就在于，一个是隐式的原型，一个是显式的原型，隐式的原型应该是私有的，但是很遗憾的是我们也是可以访问的，也是可以修改的，显式的原型同样也是可以访问可以修改的。

好的，目前我们需要理清一些概念，应为javascript比较特殊，构造函数是对象，构造函数又可以创建对象，这2个都是对象，我们区分一下，构造函数创建的对象叫普通对象，构造函数我们就要构造函数，普通对象是没有显


- 相关文档

[深入理解javascript构造函数和原型对象](http://www.jb51.net/article/55539.htm#card_1508137382789_9336)