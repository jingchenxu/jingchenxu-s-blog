## vue render 方法

- 为什么需要render

我们最常用的vue的模板写方有什么呢？大家可以参考下参考文档中提供的7中方法，但是事实上我们最常用的可能只有2中，一种是字符串，还有一种是Vue单文件的template,但是能在ES5环境下，不使用打包编译的情况下，我们的选择可能就会少一些，正常情况下用的是字符串，字符串相对来说不是一个很好的方案，但是我用的比较多，多亏了webstorm对html字符串的解析。
官方文档里有说明使用render函数能够提高性能，那就是吧！通过render可以使用纯粹的js来写html了，我们在写组件的时候，使用render函数来渲染模板还是不错的。
最近又继续了解了一下，其实组件如果使用模板的话，最后还是会被编译为render函数，所以如果你是工程化的项目，其实可以不用关心是否使用的是render函数，因为你的项目最终是会将模板编译为render函数，这一点可以在iview的组件库中看一下，iview开发的时候使用的是模板的方式，但是最后编译出来的都是render函数，这也就是说如果是模板能够实现的，render函数也能够实现，某种程度上模板可以看做一个配置文件，组件在加载时会检查是否有template,如果没有template，则检查是否有render函数，所以对于不编译的项目，使用render函数是能够提高性能的，但是对于编译的项目，其实没有区别。

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
				scopedSlots: {  // 该属性用于处理内容分发
                                title: function() {
                                   var groupTitle = h('span', {
                                       props: {
                                           slot: 'title'
                                       },
                                       domProps: {
                                           innerHTML: name
                                       }
                                   });
                                    return groupTitle;
                                }
                               }
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

在render函数中有一个默认的默认方法作为参数，通过该函数渲染组件的各个几点，将每个对象抽象为一个对象，{tagName, tagAttrs, children},可以看到该对象主要有以上的三个属性，第一个属相为标签名称，第二个属组件的一些具体属性，该属性也是一个对象，可以具体的细分，第三个属性为当前标签的子标签，概属性为一个数组，因此当前标签可以拥有多个字标签，每个字标签与当前标签一样都是一个h函数，通过一层又一次的嵌套形成一个完成的dom树。

render函数可以渲染指定的配置文件外，同时也可以直接渲染组件对象，如果有一个组件为Component,那么在直接渲染的时候可以渲染为：

````javascript
render: function(h) {
    h(Component)
}
````

此外如果你可能需要对此组件对象添加对应的属性，也可以通过局部注册组件，然后通过标签的方式进行渲染，这里就不提供代码了。

- 参考文档

[Vue.js 中，7种定义组件模板的方法 | Codementor](http://www.zcfy.cc/article/7-ways-to-define-a-component-template-in-vue-js-codementor-3644.html)