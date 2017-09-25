## 一个vue组件的基本属性

- 组件构成

一个vue组件主要包含三个部分，相比于react组件，vue组件的书写结构相对来说更为清晰，主要分为html模板，样式，javascript这三个部分。

- 组件的生命周期

如果之前用过react的人都知道一个概念，那就是组件的生命周期，它将组件的的一生划分为多个阶段，在各个阶段，都设置了一个钩子函数，这样在组件的各个阶段可以触发一些动作。

这些钩子函数经常会在哪些场景下使用呢？数据的加载，最开始用react的时候，还没有使用redux进行前端数据的管理，所以那时候的数据都是在组件渲染完成后，调用ajax获取后端数据。在单页应用当中，一个简单的用法就是可以在组件加载之前，切换页面的标题。

组件的生命周期主要可以分为以下部分：

````javascript
    beforeCreate: function () {
      console.group('beforeCreate 组件准备创建=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:blue', 'el:' + this.myname)
    },
    created: function () {
      console.group('created 组件创建完成=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    beforeMount: function () {
      console.group('created 组件准备挂载=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    mounted: function () {
      console.group('mounted 组件挂载完成=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    beforeUpdate: function () {
      console.group('beforeUpdate 组件准备更新=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    updated: function () {
      console.group('updated 组件更新完成=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    beforeDestroy: function () {
      console.group('beforeDestroy 组件即将销毁=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    },
    destroyed: function () {
      console.group('destroyed 组件销毁=====》')
      console.log('%c%s', 'color:red', 'el:' + this.$el)
      console.log('%c%s', 'color:red', 'el:' + this.$data)
      console.log('%c%s', 'color:red', 'el:' + this.myname)
    }
````







