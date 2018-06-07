# vue实现根据接口数据动态生成表单页面

> 相信这样的需求在很多实用vue的后台管理型项目中都会遇到，后台管理型应用中出现最多的是列表与详情页，详情页就是这里的表单页面，如果通过手动的编写模板的方式来实现列表页是在是太费劲了，主要是要花费较多的时间来写页面，如何简化呢，这里就想到了vue的动态组件（该实现基于iview UI组件库）。

### 具体需求

在详情页我们常常还会遇到子表的需求，一个主表可能会有多个子表，所以在详情页，我们在展示完主表信息之后，还需要列表展示子表的信息，这些信息需要可以新增、删除、修改，具体最后的效果如下：![](/img/VUE/sublistdetail.gif)该组件计划的属性有哪些呢？该组件主要有3个属性，其中2个是数据，一个是回调方法，2个数据属性为columns（表头）、data（表单数据），回调方法的作用在于每次table数据发生变化时调用，用于保持组件内部的数据与外部数据的一致性。

### 组件设计

该组件的设计需要使用到iview组件库中的 Table、Modal、Form、Button这4个组件，重点需要设计组件属性为columns属性，该属性不仅最为该组件中列表页的表头，同时还作为动态生成表单页面的数据，具体的设计可以参考如下：

```js
import moment from 'moment'
export const columns = [{
  type: 'selection',
  width: 60,
  align: 'center'
},
{
  type: 'inputitem',
  label: '测试输入框',
  prop: 'test1', // 同时其也是列表中使用的
  placeholder: '请输入测试的内容',
  title: '测试的第一个字段',
  key: 'test1',
  align: 'left',
  ellipsis: true,
},
{
  type: 'inputitem',
  label: '测试输入框1',
  prop: 'test2', // 同时其也是列表中使用的
  placeholder: '请输入测试的内容',
  title: '测试的第二个字段',
  key: 'test2',
  align: 'left',
  ellipsis: true,
},
{
  type: 'selectitem',
  label: '测试选择框',
  prop: 'test3', // 同时其也是列表中使用的
  placeholder: '请选择',
  title: '测试的第三个字段',
  key: 'test3',
  align: 'left',
  ellipsis: true,
  options: [
    {
      id: '01',
      name: '01'
    },
    {
      id: '02',
      name: '02'
    },
    {
      id: '03',
      name: '03'
    }
  ]
},
{
  type: 'datepickeritem',
  label: '测试时间',
  prop: 'test4', // 同时其也是列表中使用的
  placeholder: '请选择时间',
  title: '测试时间',
  key: 'test4',
  align: 'left',
  ellipsis: true,
  render: function (h, params) {
    let test4 = moment(params.row.test4).format('YYYY-MM-DD hh:mm:ss')
    return h('span', test4)
  }
},
{
  type: 'textareaitem',
  label: '测试文本框',
  prop: 'test5', // 同时其也是列表中使用的
  placeholder: '请输入测试的文本框',
  title: '测试的第五个字段',
  key: 'test5',
  align: 'left',
  ellipsis: true,
},
]

export const data = [{
  test1: 1,
  test2: 2,
  test3: '01',
  test4: new Date(),
  test5: 'asdfas sagfdsgfdhgfhdhg'
},
{
  test1: 1,
  test2: 2,
  test3: '01',
  test4: new Date(),
  test5: 'asdfas sagfdsgfdhgfhdhg'
},
{
  test1: 1,
  test2: 2,
  test3: '01',
  test4: new Date(),
  test5: 'asdfas sagfdsgfdhgfhdhg'
},
{
  test1: 1,
  test2: 2,
  test3: '01',
  test4: new Date(),
  test5: 'asdfas sagfdsgfdhgfhdhg'
},
{
  test1: 1,
  test2: 2,
  test3: '01',
  test4: new Date(),
  test5: 'asdfas sagfdsgfdhgfhdhg'
},
]

```

接下来设计的关键是，form表单内动态组件的设计，基于vue的动态组件实现，我们需要准备还可能用到的动态组件，保证这些组件接收参数，和对外回调的一直性，即对外对内都表现一致。

组件的源码文件目录如下：

![](/img/VUE/5Y~{M%8@4IC%R8AFV5$FA@7.png)

data中的为测试数据，formcomponents中的为进行表现一致封装的form表单组件，sublistdetail.vue 则为组件的主体，主体组件中包含了Table、Modal,没有拆分，主要考虑到降低组件的复杂性，具体的代码实现如下：

```js
<template>
  <div class="sublistdetail">
    <h1>sublistdetail</h1>
    <Card :bordered="false" :dis-hover="true" class="searchbar-container">
      <Form inline :label-width="0">
        <FormItem :label-width="0">
          <Button type="primary" @click="handleAdd" icon="android-add">新增</Button>
          <Button type="error" @click="handleDelete" icon="ios-close">删除</Button>
        </FormItem>
      </Form>
    </Card>
    <whitespace :spaceheight="10"></whitespace>
    <Table @on-row-dblclick="dbclick" @on-selection-change="selectionChange" border stripe size="small" :columns="columns" :data="listData"></Table>
    <Modal :mask-closable="false" v-model="modal" title="新增明细">
      <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
        <component v-if="item.type !== 'selection'" v-for="(item, index) of columns" :formValidate="formValidate" :key="index" :is="item.type" :itemconfig="item" @valueChange="valueChange"></component>
        <FormItem>
          <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
          <Button type="ghost" @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
        </FormItem>
      </Form>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>

<script>
import * as formcomponents from './formcomponents'
import { whitespace } from '../listdetail'
import utils from '@/utils/utils.js'
import _ from 'lodash'

export default {
  name: 'sublistdetail',
  props: {
    columns: {
      type: Array,
      required: true,
    },
    data: {
      type: Array,
      required: true
    }
  },
  components: { ...formcomponents, whitespace },
  data () {
    return {
      loading: false,
      modal: false,
      selection: [],
      listData: [],
      formValidate: {},
      detailStatus: 'add', // add || edit
      ruleValidate: {
        test1: [{ required: true, message: 'test1不能为空', trigger: 'blur' }],
        test5: [{ required: true, message: 'test5不能为空', trigger: 'blur' }]
      }
    };
  },
  mounted () {
    let me = this
    let {data} = me
    for (let i = 0; i < data.length; i++) {
      let item = data[i]
      item['_index'] = i
      this.listData.push(item)
    }
  },
  methods: {
    handleSubmit (name) {
      let me = this
      this.$refs[name].validate(valid => {
        if (valid) {
          let formValidate = me.formValidate
          // 判断是编辑还是新增
          if (formValidate._index !== undefined) {
            // 在数组中找到它
            for (let i = 0; i < me.listData.length; i++) {
              if (me.listData[i]._index === formValidate._index) {
                for (let key in formValidate) {
                  me.listData[i][key] = formValidate[key]
                }
              }
            }
          } else {
            formValidate['_index'] = me.listData.length
            me.listData.push(formValidate)
          }
          me.formValidate = {}
          me.handleReset(name)
          me.$emit('afterAdd', me.listData)
          this.$Message.success('success')
          me.modal = false
        } else {
          this.$Message.error('error')
        }
      });
    },
    handleReset (name) {
      this.$refs[name].resetFields();
    },
    dbclick (row, index) {
      this.modal = true;
      this.formValidate = row
    },
    handleAdd () {
      this.modal = true
      this.detailStatus = 'add'
      this.formValidate = {}
    },
    handleDelete () {
      let me = this
      let {selection} = this
      if (selection.length === 0) {
        this.$Message.error('请选择需要删除的数据')
      } else {
        for (let item of selection) {
          utils.removeObjWithArr(me.listData, item, '_index')
        }
        me.$emit('afterDelete', me.listData)
        me.detailStatus = 'edit'
        me.selection = []
      }
    },
    selectionChange (selection) {
      this.selection = selection
    },
    valueChange (key, value) {
      this.formValidate[key] = value
    }
  }
};
</script>

<style scoped>
</style>

```

这里面有个有意思的地方，让我对vue中的数据双向绑定有了更进一步的认识，下面的这段代码：

```js
if (me.listData[i]._index === formValidate._index) {
for (let key in formValidate) {
me.listData[i][key] = formValidate[key]
}
}
```

如果使用下面的这段代码会怎样：

```js
if (me.listData[i]._index === formValidate._index) {

me.listData[i] = formValidate

}
```

你会发现，页面并没有因为数据的改变而跟着改变，简单的来说，通过对数据中某个对象的赋值，不会使该对象变为可观察的对象，则不会实现数据双向绑定，这部门的内容找个时间具体的验证一下。

下面以input输入框为例进行动态组件的封装：

```js
<template>
  <FormItem :label="itemconfig.label" :prop="itemconfig.prop" :labelWidth="100">
    <Input @on-change="valueChange" v-model="value" :placeholder="itemconfig.placeholder"></Input>
  </FormItem>
</template>

<script>
export default {
  name: 'inputitem',
  props: {
    itemconfig: {
      type: Object,
      required: true
    },
    formValidate: {
      type: Object
    }
  },
  data () {
    return {
      value: ''
    }
  },
  watch: {
    formValidate: {
      handler: function (curVal, oldVal) {
        let {key} = this._props.itemconfig
        if (!curVal[key]) {
          this.value = ''
        } else {
          this.value = curVal[key]
        }
      },
      deep: true
    }
  },
  methods: {
    valueChange () {
      console.log(this.value)
      this.$emit('valueChange', this._props.itemconfig.key, this.value)
    }
  }
}
</script>

```

所有组件输入一致，输出一致，以上则是表单页面动态生成的思路。

