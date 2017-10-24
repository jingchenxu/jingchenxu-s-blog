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