# 链式调用

* 先实现一个简单的链式调用

jquery中可以实现获取dom元素后，通过链式调用设置该dom对象的说个属性，代码清晰连贯，可以实现更好的语意，下面我们就实现一个简单的链式调用：

```javascript
        var Chain = function(){
            this.get1 = function(a1){
                this.a1 = a1;
                return this;
            }

            this.get2 = function(a2){
                this.a2 = a2;
                return this;
            }

            this.get3 = function(a3){
                this.a3 = a3;
                return this;
            }
        }

        var chain = new Chain();
        console.dir(chain.get1(1).get2(2).get3(3));
```

这段代码的主要作用是设置一个对象的三个属性，如果不使用链式调用，可能需要使用3个语句才能实现此功能，但是通过链式调用，一个语句就可以清晰的实现此功能。

* Promise中的链式调用

