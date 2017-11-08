## vue render 方法

- 为什么需要render

我们最常用的vue的模板写方有什么呢？大家可以参考下参考文档中提供的7中方法，但是事实上我们最常用的可能只有2中，一种是字符串，还有一种是Vue单文件的template,但是能在ES5环境下，不使用打包编译的情况下，我们的选择可能就会少一些，正常情况下用的是字符串，字符串相对来说不是一个很好的方案，但是我用的比较多，多亏了webstorm对html字符串的解析。
官方文档里有说明使用render函数能够提高性能，那就是吧！通过render可以使用纯粹的js来写html了，我们在写组件的时候，使用render函数来渲染模板还是不错的。

- render的写法

创建一个最简单的Vue组件，该组件中不集成其他的组件：

````javascript
var OriginalPage = Vue.extend({
	name: 'OriginalPage',
	data: function () {
		return {}
	},
	directives: {
		jcxu: {
			inserted: function (el, binding) {
				//alert('insert');
				alert(el);
				alert(binding);
			}
		}
	},
	render: function (h) {
		console.dir(arguments);
		return h(
			'h'+this.level,
			{
				'class': {// 和v-bind:class 一样的属性
					show: true,
					hide: false
				},
				style: {  // 和v-bind:style 一样的属性
					width: '200px',
					height: '400px',
					background: 'red'
				},
				attrs: {  // 正常的html标签属性
					name: 'h-ex',
					id: 'h-id'
				},
				props: {  // 组件props
					myprops: true
				},
				// domProps: {//DOM属性
				// 	innerHTML: 'baz'
				// },
				// 事件监听基于 on
				// 所以不再支持如 v-on:keyup.enter 修饰器
				// 需要手动匹配 keyCode
				on: {
					click: function (event) {
						alert(this)
					}
				},
				nativeOn: {
					click: function (event) {
						alert(1111);
					}
				}
				// 自定义指令 不能对绑定的旧值进行设置
				// Vue 会为您持续追踪
				directives: [
				 	{
				 		name: 'jcxu',
				 		value: this.onJcxu
				 	}
				 ]
			},
			// 此处存放的是子节点
			[
				this.$slots.myslot,
				h('div', {
					domProps: {
						innerHtml: 'hello render'
					}
				})
			]
		)
	},
	props: {
		level: {
			type: Number,
			required: true
		}
	}
});
````

- 参考文档

[Vue.js 中，7种定义组件模板的方法 | Codementor](http://www.zcfy.cc/article/7-ways-to-define-a-component-template-in-vue-js-codementor-3644.html)