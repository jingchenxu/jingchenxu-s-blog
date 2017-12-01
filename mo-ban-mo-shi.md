## 模板模式

>在写一个react组件或是vue组件的时候，我们写一个组件的格式是固定的，是不是很像是在写一个配置文件，我们可以看一下react早期的组件代码和vue的组件代码。

react

````javascript
React.createClass({
    displayName: 'demo',
    render: function () {
        return (
            <div></div>
        )
    }
})
````

vue

````javascript
Vue.extend({
    name: 'demo',
    template: '<div></div>',
    data: function () {
        return {
        }
    }
})
````

我们自己创建一个简单的模板

````javascript
    window.onload = function(){
        /*
        @author jcxu
        @description 创建一个组件模板
        */
        var jcxu = function(config) {
            var el = config.el || '#app';

            // 先找到组件挂载点
            var getEl = function() {
                console.log('挂载点为'+el);
            };

            var step1 = config.step1 || function () {
                throw new Error('必须重写step1f方法');
                return false;
            }

            var step2 = config.step2 || function () {
                throw new Error('必须重写step2方法');
                return false;
            }

            var F = function() {};

            F.prototype.init = function() {
                // 按照顺序执行相应的方法
                getEl();
                step1();
                step2();
            };

            return F;
        }


        var demo = jcxu({
            el: '.app',
            step1: function() {
                console.log('此方法已被重写');
            }
        });

        var demo1 = new demo();
        demo1.init();
    }
````