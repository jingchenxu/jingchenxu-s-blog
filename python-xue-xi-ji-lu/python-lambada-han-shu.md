# python lambada 函数

- lambada应用在函数式编程当中

这应该时lambada表达式最常见的使用场景了，将函数作为参数传入到函数当中，在node, java中这是最长使用到lambada函数的地方，常见的一些函数如：map、reduce、filter、sorted这些函数都接受函数作为参数，可以参考下下面的代码：

````python
list = [3, 5, -4, -1, 0, -2, -6]
sorted(list, key=lambda x: abs(x))
````