## React 开发注意事项

#### 组件引入注意

1、对于新手可能在引入react组件的时候就会遇到问题，遇到的第一个问题可能是你的组件名称没有小写，react组件的名称是要大写的；

#### 页面元素循环遍历注意

1、在react中经常会用到map 根据数据遍历出组件，这在react中经常会用到，react为了更高效的渲染，会要求你为每个组件添加key,用于优化渲染，很多的时候我们不是很在意，都会直接使用，map中的i作为key值，正常情况下是没什么问题的，但是在一些特殊的情况下，比如购物车页面是，可能会导致一些问题，一种不负责任的解决方案是使用Math.radom()作为key值，但是更优的解决方案是，使用实际业务中主键值作为key值；

#### onscroll事件监听注意

1、如果你在一个有状态的组件中需要用到事件简体，你可以在componentDidMount 生命周期中添加事件监听，但是别忘了在componentDidUmmount 生命周期中移除事件监听；

#### 组件完全强制刷新方法

1、在某些情况下列表组件在数据修改时页面刷新不完全，或是无法触发组件的生命周期钩子函数，在这种情况下，我们想要强制刷新组件的话，可以使用修改组件的key值，修改key值会使组件重新加载；

#### 数据多层嵌套时的注意

#### 前端高精度计算问题

#### 子组件向父组件通信传值

在使用了redux 和 DVA 之后，我很久没有涉及到子组件向父组件通信的问题了，但是对于纯粹的数据展示类的项目，完全是可以不适用这些框架的，但是带来的问题就是后期对项目的扩展不是很方便，见识还是使用redux吧！子组件向父组件通信多半使用的是回调函数，目前主要在项目组件的开发中会用到，采用组件内部状态机制，可以降低组件的耦合度，方便在不同的项目中使用；

#### 延时查询

#### 公共调用组件

#### 判断浏览器

```javascript
judgeClient: function () {
		let u = navigator.userAgent,
			app = navigator.appVersion;
		return {
			trident: u.indexOf('Trident') > -1, //IE内核
			presto: u.indexOf('Presto') > -1, //opera内核
			webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
			gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
			mobile: !!u.match(/AppleWebKit.*Mobile.*/), //是否为移动终端
			ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
			android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或uc浏览器
			iPhone: u.indexOf('iPhone') > -1 , //是否为iPhone或者QQHD浏览器
			iPad: u.indexOf('iPad') > -1, //是否iPad
			webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
		}
	}
```

### 体验优化

#### 图片惰性加载

#### 图片加载出错设置默认