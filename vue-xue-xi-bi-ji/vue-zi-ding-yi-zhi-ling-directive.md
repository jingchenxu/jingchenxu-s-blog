# vue 自定义指令 directive

## directive 的基本API

基本的api在官方文档上也有，这里也简单的介绍下，一个指令定义对象可以提供如下的几个钩子函数，不一定都要触发，根据自身的需求进行触发：

* `bind`：只调用一次，指令第一次绑定到元素的时候进行调用；
* `inserted` ：被绑定的元素插入到父节点的时候；
* `update`：组件VNode发生更新时调用；
* `componentUpdated`：指令所在的组件VNode及其子VNode全部更新后调用；
* `unbind`：指令与元素解绑时进行调用。

具体的写法可以参考如下的代码：

```javascript
  Vue.directive('errorimg', {
    bind (el, binding, vnode, oldvnode) {
    },
    inserted: function (el) {
      console.dir(el)
    },
    update (el) {
      console.dir(el)
    },
    componentUpdated (el) {
      console.dir(el)
    },
    ubbind (el) {
      console.dir(el)
    }
  })
```

参数说明：

* `el`：指令所绑定的元素，可以用来直接操作 DOM 。
* `binding`：一个对象，包含以下属性：
  * `name`：指令名，不包括 v- 前缀。
  * `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  * `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  * `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  * `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  * `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
* `vnode`：`Vue` 编译生成的虚拟节点。移步 VNode API 来了解更多详情。
* `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。





