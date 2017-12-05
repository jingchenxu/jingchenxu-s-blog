## javascript 的constructor

- constructor是什么

是的，你想问他是谁？这是什么？constructor是什么？javascript对象中的constructor属性指向创建该对象的构造函数，但是需要注意的是，对象的constructor是可以修改的，所以说constructor不是绝对指向创建该对象的构造函数的。

- constructor可以做什么

让我们先看一段代码：

````javascript
        var object = new Object();
        // 这里我们可以清楚的看到object的构造函数为Object()
        console.dir(object.constructor);
        var Demo = function() {
            this.myName = 'xu';
            this.age = 26;
        },
        demo1 = new Demo();
        // 这里我们可以看到demo1的构造函数为Demo()
        console.dir(demo1.constructor);
        console.dir(demo1);
        var demo2 = Object.create(demo1);
        // 这里的demo2是通过复制demo1得到的
        console.dir(demo2.constructor);
        // javascript中所有的对象都是基于原型的，所谓的基于原型，就是对象的创建是通过复制扩展对象的方式而实现的
        // 所谓的构造函数是通过复制或扩展基础函数从而创建对象的函数
        // 构造函数Demo()可以看作是通过复制或扩展{myName: 'xu',age: 28}实现的
        // 通过修改基础对象我们可以为类（构造函数Demo())创建的对象添加属性
        // 这里是原型链的体现
        demo2.constructor.prototype.fullName = 'xujingchen';
        // demo1和demo2都添加了fullName属性
        console.dir(demo1);
        console.dir(demo2.fullName);

        // 我们在无法直接修改构造函数的时候，可以通过对象的constructor属性修改构造函数
        function getTest() {
            var Test = function() {
                this.test1 = 123;
                this.getTest2 = function() {
                    console.log('this is test2' + this.test2);
                }
            }

            var temp = new Test();
            return temp;
        }

        var newTest = getTest();
        // 我们可以看到创建的对象没有test2属性，很多的时候各类的框架都是通过闭包函数进行封装的
        // 我们如何在不改变框架的前提下，为框架类添加添加属性，这里可以看做一种AOP的思想
        newTest.getTest2();
        // 之后所有通过闭包函数内的类创建出来的对象都有此属性。
        newTest.constructor.prototype.test2 = 'new name';
        newTest.getTest2();

        // 通过newTest的构造函数创建一个新的对象
        var newTest1 = new newTest.constructor();
        console.dir(newTest1);
````

通过上面的代码并配合注释，我们可以理解以下的知识：

1. javascript对象constructor指向其构造函数，及其的一些基本用法；

2. constructor较多的应用场景是在构造函数在闭包函数中（多见于框架封装为模块），无法直接访问时，通过修改构造函数的显式原型，可以为构造函数创建的对象添加属性；同时也可以直接使用对象的构造函数创建对象。

- TODO

在写这篇文章的时候发现这里的constructor与java中AOP，反射等方面有一定的类似，以后在详细的对比下。