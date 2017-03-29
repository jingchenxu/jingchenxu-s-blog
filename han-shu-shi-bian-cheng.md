## 函数式编程

### 函数式编程是什么鬼

至于函数式是什么鬼，我开始也是不知道的，我注意到函数式编程是在github上看到别人的代码，在react组件中别人的样式是这么写的，在需要的时候调用该函数：

```javascript
function getStyles() {
	return {
		itemContainer: {
			width: '100%',
			height: 'auto',
			marginBottom: '20px',
			padding: '0 20px 0 20px',
			backgroundColor: 'white',
			overflow: 'hidden'
		}
	}
}
```

而在看到别人这么写之前，都是在组件的外部或是内部定义一个变量，这个变量即为style样式。

```javascript
const style = {
itemContainer: {
			width: '100%',
			height: 'auto',
			marginBottom: '20px',
			padding: '0 20px 0 20px',
			backgroundColor: 'white',
			overflow: 'hidden'
		}

}

```

我不禁思索，这样写的好处是什么，其实我也不知道有什么好处，写到现在，原来的写法也没出现过什么问题，参考文档1中提出了函数式编程的几点优势，作用域局限，副作用少，这一点我是认同的，我们就上面的样式的写法分析一下，函数式编程的作用域局限，副作用少的优点：

如果采用变量定义样式的方式，如果该变量定义在组件的外部，那么在整个组件中都是可以访问的，此外我们可能在组件内部继续定义变量，重名也是一个问题，而如果采用函数式的编程方式，则可以避免掉这些问题。

其实上面的函数式编程就是一个闭包函数，闭包函数的一个特性就是在闭包函数中的变量会被保存在内存当中，在使用函数式声明样式的时候，页面中多个地方调用样式，那么保存在内存中无疑是比较好的，而采用变量的话可能就需要调用多次。

参考文档2中有一道很有意思的题目：

```javascript
var name = "The Window";   
　　var object = {   
　　　　name : "My Object",   
　　　　getNameFunc : function(){   
　　　　　　return function(){   
　　　　　　　　return this.name;   
　　　　　};   
　　　　}   
};   
alert(object.getNameFunc()());  //The Window
```

我扫了一眼当时就认为答案必然是"My Object"，但是很快就被啪啪打脸了，其实是可以理解的，我少看了this.name前面还有个return，第一个return的是一个匿名函数，return将这个匿名函数暴露在了上面的一段代码当中，那么这个匿名函数的作用域就应该是```var name = "The Window";``` 所在的作用域，那么this 则不是指 ```object``` ,而是```var name = "The Window";```所在的this。

个人感觉函数式编程，有利于模块的封装，首先函数式的模块对外表现的可以被任意的命名，在react中，目前倡导的无状态组件，本质也是函数式编程，无状态组件声明的是一个方法，而另外的2中有状态的组件，一个可以是认为是一个组件对象，而采用class这样的概念声明的组件本质上也是一个组件对象。

### 纯函数

首先纯函数又是什么？

1、 不触碰外部的变量，下面代码中的z不被add所接触及修改：

```javascript
var z = 10;
function add(x,y) {
    return x+y;
}
```

2、 固定的输入对应固定的输出，如果你的输入值是相同的，那么无论你输入多少次，只要是相同的输入值，那么输出值都应该是不变的。

3、 在纯函数的内部没有除了输入的变量以外的变量，所以在纯函数的内部像for循环这样的是不能用的，for循环中申明的循环用的变量，就是输入的变量以外的变量，那么在函数式编程中是如何实现循环的呢？答案是递归，下面抄一段别人的代码：

```javascript
//A:simple loop cinstruct
var add = 0;
for (var i = 1; i <= 10; i++)
  acc += i;
console.log(acc);//prints 55
//B:without loop construct or variables (recursion)
function sumRange(start, end, add) {
  if (start > end)
    return acc;
  return sumRange(start + 1, end, acc + start)
}
console.log(sumRange(1, 10, 0));//prints 55
```

A方案是我们正常通过for进行循环，而B方案中循环则是通过递归调用进行的对于sumRange这个函数来说其每次调用的时候来说输入的值是固定的，那么其输出就是固定的。（通过结果推到出的过程，理解没透彻。）

### 参考文档

[1][我眼中的 JavaScript 函数式编程](https://zhuanlan.zhihu.com/p/25985501)
[2][javascript深入理解js闭包](http://www.jb51.net/article/24101.htm)