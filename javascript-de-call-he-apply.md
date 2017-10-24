## javascript 的call与apply

- 起因

我们知道arguments对象是函数中的参数对象，他很像一个数组，但是他并不是一个数组，在javascript中有很多这样的问题，我们可以看到下图中的函数参数数组和平常的数组类型是不相同的。

````javascript
        function add(a, b) {
            console.log(arguments);
        }
        add(5, 7);
        var test = [5, 7];
        console.log(test)
````

![此处显示的是如何设置的图片](/img/javascript/arguments.png)

![此处显示的是如何设置的图片](/img/javascript/array.png)

通过上面的对比我们可以明显的看到








