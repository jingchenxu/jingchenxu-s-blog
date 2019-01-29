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

这个是个比较特殊的地方，







