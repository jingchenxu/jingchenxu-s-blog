## vue 的 v指令

很多人喜欢vue的时候回提到一个原因就是，html模板与js的分离，在react中你可以通过jsx实现在页面中写代码的小心思，有人说这是因为facebook php起家的原因导致的，作为一个react、vue都是用的人来说，俩者各有各的好，俩个我都喜欢。

react中的页面组件的循环加载使用过map函数来实现的，那么在vue中是如何实现的呢？

下面我们来一一探讨vue的v-xxx指令：

- v-bind && v-for

首先说一下v-bind的作用，我们在使用一个vue组件时，需要向组件的内部传入一些属性，这时候就需要用到v-bind命令，需要注意的是在较新的vue版本中，v-bind的指令可以简写为 ：；

我们在应用中常常会遇到一些列表，需要循环遍历组件，在react当中我们使用的是map函数，在vue中我们使用v-for指令，在vue较新的版本中，已经集成了虚拟dom,所以在使用v-for指令时注意给被循环组件添加key属性。

具体使用示例如下：

````vue
<template>
  <div>
    <VbindItem
    v-for="item in todolist"
    :item="item"
    :key="item.id"></VbindItem>
  </div>
</template>

<style lang="css" scoped>
  
</style>

<script>
import VbindItem from '@/components/VbindItem'

export default{
  name: 'vbinddemo',
  components: { VbindItem },
  data () {
    return {
      message: 'goodmessage',
      todolist: [
        {id: '01', name: 'test1'},
        {id: '02', name: 'test2'},
        {id: '03', name: 'test3'},
        {id: '04', name: 'test4'}
      ]
    }
  }
}
</script>
````

````vue
<template>
  <div class="vbinditem">
    {{item.name}}
  </div>
</template>

<style lang="css" scoped>
  .vbinditem {
    width: 200px;
    height: 50px;
    line-height: 50px;
    border-bottom: 1px solid red;
    text-align: center;
  }
</style>

<script>
  export default {
    name: 'VbindItem',
    props: ['item'],
    data () {
      return {

      }
    }
  }
</script>
````

- v-on

v-on指令主要用于dom事件的监听作用，vue与react都对原生的dom事件进行了封装。

v-bind 的指令需指明，监听的事件类型，是点击事件（click）,还是change事件，具体代码如下：

````html
<template>
  <div v-on:click="itemclick(item.name)" class="vbinditem">
    {{item.name}}
    <el-input v-on:change="itemchange()"></el-input>
  </div>
</template>
````

````js
    methods: {
      itemclick (name) {
        console.log('被点击了' + name)
      },
      itemchange () {
        console.log('see')
      }
    }
````
