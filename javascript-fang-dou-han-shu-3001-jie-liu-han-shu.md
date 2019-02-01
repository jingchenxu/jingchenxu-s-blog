# JavaScript 防抖函数、节流函数

## 函数防抖

目前我遇到的函数防抖的场景是防止重复提交，代码如下：

````javascript
let timer = null;

function debounce(fn, delay) {
  return function () {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      console.dir(arguments)
      fn.apply(this, arguments);
    }, delay);
  }
}

function showLog(value) {
    console.log('input'+value)
}

debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')
debounce(showLog, 1000)('jcxu')

setTimeout(function () {debounce(showLog, 1000)('jcxu')}, 800)
setTimeout(function () {debounce(showLog, 1000)('jcxu')}, 2000)
````

