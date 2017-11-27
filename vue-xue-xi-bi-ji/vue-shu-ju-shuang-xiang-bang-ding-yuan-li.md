## vue 数据双向绑定原理

- MVVM

vue是一个MVVM架构的前端框架，可以理解为view 、model 、 model and view ,view可以理解为vue的模板，model为数据交互层，这部分主要体现在vue组件的 methods、生命周期函数上，这些属性中会发生于后端的一些列数据交互，model and view则主要体现在 vue组价的data、computed、watch这些属性上，这些属性中的数据都是与视图惊醒双向绑定的。

- 数据双向绑定解决了什么问题

然我们回到很久以前，一个大家普遍使用jQuery的时代，在这个时代，通过ajax或是后端模板引擎的渲染我们获取到数据，在html的表单中修改了数据后，我们是如何提交到后台的呢，我们需要用js搜集页面的数据，然后再通过ajax将数据发送到后台，如果后台数据发生了变化，我们通过ajax请求获取到了数据，但是还是需要通过js将获取到的数据设置到页面上。

其实上面的操作是有共性的，且非常的重复，之前在使用react写项目的时候，react实现了数据的单向绑定，想要实现，react组件的state与页面数据的同步需要用到回调函数来setState,其实这样我都是闲麻烦的，如果能在页面数据改动的时候自动的setState,通过setState改变state的同时，触发了react虚拟DOM的更新，从而更新视图。

我们需要清楚，页面的修改必然是setState触发的，我原本因为在数据变化的时候触发的，可参见[为什么一定要通过setState修改状态](https://segmentfault.com/q/1010000009073986)，所以我们直接修改state是不会修改视图的。如何让我们在直接修改完state刷新视图呢？我偶然看到了这个[link](https://github.com/lhang/blog/issues/3),数据的双向绑定简化了我对页面数据的管理，不需要频繁的手动获取和设置页面数据，数据即视图，视图即数据。

- vue如何实现数据的双向绑定

blackbone使用的是发布订阅模式，视图订阅了数据改变的事件，数据改变后，视图受到通知，数据订阅了视图改变事件，视图改变后，数据接收到通知。

vue利用了javascript对象的一个属性，该属性IE8不支持，这也是vue不支持IE8的原因，我们在数据发生变化的时候，要能够通知别人，也就是说在obj.attr1=1,这个操作执行的时候能够主动的通知别人，而defineProperty可以在对对象数据修改时做一层拦截，有点类似拦截器的思想，

![](/img/VUE/define.png)

- 参考文档

[剖析Vue原理&实现双向绑定MVVM](https://segmentfault.com/a/1190000006599500)
