## javascript 创建对象的几种方法

- 使用字面量创建对象

````javascript
        var object0 = {
            name: 'object',
            start: function() {
                console.log('开始了');
            },
            pause: function() {
                console.log('暂停');
            },
            stop: function() {
                console.log('结束');
            },
            _init_: function() {
                this.start();
                this.pause();
                this.stop();
            }
        }
````

可以看到如果使用字面量来创建对象，每创建一个对象就要写这么一大通的代码，还是比较烦的

- 使用Object构造函数来创建一个对象

````javascript
        var object1 = new Object();

        object1.name = 'object1';
        object1.start = function () {
            console.log('start...');
        };
        object1.pause = function () {
            console.log('pause....');
        };
        object1.stop = function () {
            console.log('stop...');
        };
        object1._init_ = function () {
            this.start();
            this.pause();
            this.stop();
        }
````

其实使用字面量与使用Object构造函数来创建对象的区别不是很大，过程相对来说都比较的繁琐，如果创建同一类型的对象的话，需要些一大堆的无用代码。

我们看到可以使用Object构造函数创建对象，创建出来的是一个基本的对象，然后可以在这个对象上面通过赋值之间创建属相，我们能否使用构造函数批量创建出对象呢？

- 构造函数创建对象

````javascript
        function Component(name, start, end) {
            this.name = name;
            this.start = start;
            this.end = end;
            this._init_ = function() {
                console.log(this.name);
                console.log(this.start);
                console.log(this.end);
            }
        }

        var object2 = new Component('许静晨', '1992', '~');
````

我们可以看到此时我们可以批量的创建对象，通过构造函数的方式创建对象已java通过类创建对象的方式十分的类似。

- 工厂模式创建对象

````javascript
        // 工厂模式创建对象
        function makeObject(name, start, end) {
            var o = new Object();
            o.name = name;
            o.start = start;
            o.end = end;
            o._init_ = function(){
                console.log(o.name);
                console.log(o.start);
                console.log(o.end);
            };
            return o;
        }
        
        var object3 = makeObject('许静晨', 1992, '~');
````

工厂模式创建对象的本质其实还是通过字面量或是Object构造函数进行对象的创建，但是其通过函数参数接受对象属性值，通过return值返回创建完成的对象，因此可以批量的创建对象。



 

