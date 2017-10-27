## javascript的继承

- 通过原型进行继承

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