# v-model 指令

> v-model是个有趣且实用的指令，但是其本质就是个语法糖。

## 如果我们不使用v-model

目前主流的前端框架其实大部分都是单向数据流，数据的展示，是数据到视图的流转，这一部分没有问题，关键是如何将视图中修改的数据反馈到数据当中，一般我们使用的是回调函数，监听页面数据发生变化，然后通过回调函数将数据返回到数据对象当中，使用过react的朋友应该比较清楚这一点，在vue中有v-model所以会感触不深。

v-model的本质

```javascript
<custom-input
  :value="something"
  @input="value => { something = value }">
</custom-input>
```

所以如果我们自己编写的组件当中使用了v-model指令，如何将组件内部中值返回到组件内部呢？则需要在组件内部触发input 回调函数。

