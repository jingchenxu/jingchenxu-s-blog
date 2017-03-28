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




### 参考文档

[1][我眼中的 JavaScript 函数式编程](https://zhuanlan.zhihu.com/p/25985501)