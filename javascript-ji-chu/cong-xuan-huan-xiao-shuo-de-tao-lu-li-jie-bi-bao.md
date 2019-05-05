# 从玄幻小说的套路理解闭包

> 看过玄幻小说的人基本都会知道玄幻小说中一个演化自我世界的设定，就是主角开始创世界了，新创建的世界位于现有的世界当中，但是现有世界无法触及到新创建世界的内部，主角可以bug般的躲进去，之前写过一篇关于闭包的文章了，主要是在实际项目中遇到了一个问题，然后开始揭露闭包的真面目。

* 从网上找来的一个清晰的定义

**闭包可以让一个函数访问并操作其声明是的作用域中的变量和函数，并且，即使声明时的作用域消失了，也可以调用。**

针对上面的这样的定义，我们可以用下面的代码演示一下：

```javascript
var outerVal = 'lionel';
var later;
function outerFn(){
  var innerVal = 'karma';
  function innerFn(){
    console.log(outerVal, innerVal);
  }
  later = innerFn;
}
outerFn();  // 此时outerFn的作用域已经消失了
later();  // lionel karma

作者：lionel爱学习
链接：https://juejin.im/post/5cabde85e51d456e6e3891e0
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

虽然外面的世界已经毁灭，但是主角新创建的世界依旧存在。

**需要注意的是，闭包里的信息会一直保存在内存当中。解决的方法清除引用**

