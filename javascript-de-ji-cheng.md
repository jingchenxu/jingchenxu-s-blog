## javascript的继承

- 通过原型实现继承

````javascript
        // 先创建一个构造函数
        function Cons1() {
            this.a = 1,
            this.b = 2
        }

        // 再创建一个构造函数
        var Cons2 = function() {
            this.c = 3,
            this.d = 4,
            this.getA = function(){
                console.log(this.a);
            }
        }

        Cons2.prototype = new Cons1();

        var demo = new Cons2();
        demo.getA();
````

我们可以看到其实是构造函数继承了对象的属性，由于javascript的特殊性，函数也是对象，所以在类比java的时候，会发现，对象的概念是可以类比的，但是用构造函数类比类的时候，总有点不合适。

所以我们在看上面的代码的时候，会有那么点的疑惑，一个类继承了对象，或者更准确的说法是2个对象的融合。通过融合生成新的构造函数对象，该构造函数创建的对象中包含了被继承对象的属性。

- 通过构造函数实现继承

获取通过构造函数实现的继承更符合java的那套思想，构造函数实现的继承是通过call获取被继承对象的所有属性，并添加到继承对象当中，具体实现代码如下：

````javascript
        function Cons3() {
            this.a = 1;
            this.b = 2;
        }

        function Cons4() {
            Cons3.call(this);
            this.getB = function(){
                console.log(this.b);
            }
        }

        var demo3 = new Cons4();
        demo3.getB();
````

