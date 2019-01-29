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





