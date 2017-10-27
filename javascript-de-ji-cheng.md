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

所以我们在看上面的代码的时候，会有那么点的疑惑，一个类继承了对象，或者更准确的说法是2个对象的融合。