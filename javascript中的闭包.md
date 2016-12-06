## javascript中闭包

closure 百度了一下，闭包的英文是这样的，至于中文为什么翻译为闭包，不是很了解。

好，下面我们来开始了解一下闭包，想了解很久了。

网上看了一下，闭包主要是为了实现：1、获取函数内的局部变量；2、并这些变量始终保存在缓存中。

```javascript
var n = 999;
function shown() {
    alert(n);
}
shown();
```
上面的这段代码中，我们知道有一个变量n，我们造了一个展示它的方法shown,工具造完了，我们便开始使用。

那么有没有这样的情况呢？

```javascript
function f() {
    n=999;
    function shown(){
        alert(n);
    }
}
```
这个时候n是属于f()这个函数的