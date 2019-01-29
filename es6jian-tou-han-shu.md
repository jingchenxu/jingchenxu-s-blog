# ES6箭头函数

> 最近打算进一步的重构现有的项目，当然项目一直在重构，这批次的重构主要是针对ES6箭头函数的。

## 箭头函数省略的写法

我们可以看到的箭头函数的最简单的写法,下面是一个判断数字大小的函数：

````javascript
let test = number => number > 5
undefined
test(6)
true
test(4)
false
````

如果我们不使用箭头函数，那么代码如下：

````javascript
let test = function (number) {if(number>5){return true}else{return false}}
undefined
test(6)
true
test(4)
false
````
我们可以看到代码多了很多，再非箭头函数的写法当中，需要你手动去进行return返回值，目前看起来比较的简单，但是你需要注意以下2点：

参数只有一个的时候，可以省略括号；
返回值只有一个的时候可以省略大括号。

## 箭头函数绑定this

说到箭头函数自动绑定this,我们要先说说为什么需要箭头行数能够自动绑定this的功能，通常情况下，this是谁离的近就指向谁(这一条对于常规对象、原型链、getter & setter等都适用)；构造函数下，this与被创建的新对象绑定；DOM事件，this指向触发事件的元素。

- 严格模式与非严格模式下的this指向

这个是个比较特殊的地方，在同步代码下，非严格模式下，this是指向window的，但是在严格模式下，this为undefined:

````javascript
function f1() {
    console.dir(this)// window对象
}
function f2() {
    "use strict";
    console.dir(this)// undefined
}
````

- 异步代码中的this指向

在vue中，ajax请求数据完成后，将返回数据赋值给组件状态，在ajax的回调函数中我们需要找到组件实例，及this,如果我们使用非箭头函数，会发现我们无法找到组件实例，在回调函数的内部this是指向window的：

````javascrit
this.$axios.get('url', function () {
    console.log(this)// window
})
````

以前我们是怎样解决这个问题的呢？主要是通过bind(this)和将this赋值给一个可访问的变量，这2中方法都是为了能让我们找到this：

````javascript
let me = this
this.$axios.get('url', function () {
    console.dir(this)// vue实例
})
````

````javascript
this.$axios.get('url',function () {
     console.dir(this)// vue实例
}.bind(this))
````

````javascript
this.$axios.get('url',() => {
     console.dir(this)// vue实例
}.bind(this))
````

上面三段代码的效果都是一样的，箭头函数的自动绑定this的功能，或许就是为了解决我们无法找到的this和无处安放的灵魂。

















