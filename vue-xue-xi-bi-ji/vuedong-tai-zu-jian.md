# vue 动态组件

## 为什么需要动态组件

存在这样的需求，需要制作一个搜索栏，这个搜索栏中的字段可能是input输入框，可能是select选择框，如果是通过template模板这样的需求肯定是可以完成的，但是我们在很多的地方上需要这样的组件，能否通过传递配置数组，就能够自动的生成搜索栏成为我的探索。

## 通过v-if条件判断制作动态组件

我们需要创建一个通用的组件，通过配置文件传过来的组件类型，在v-if的判断需要选择的组件，代码如下，这里编写的是一个通用的表单输入组件：

```javascript
<template>
    <span class="searchitem">
        <FormItem v-if="searchdetail.type === 'text'" :label="searchdetail.label" prop="user">
            <Input @on-change="valueChange" type="text" :placeholder="searchdetail.placeholder">
            </Input>
        </FormItem>
        <FormItem v-if="searchdetail.type === 'select'" :label="searchdetail.label" :label-width="searchdetail.labelwidth" prop="user">
            <Select @on-change="valueSelect" :placeholder="searchdetail.placeholder" :style="searchdetail.class">
                <Option :key="index" v-for="(item, index) in searchdetail.options" :value="item.value">{{item.name}}</Option>
            </Select>
        </FormItem>
        <FormItem v-if="searchdetail.type === 'date'" :label="searchdetail.label">
            <DatePicker v-model="defaultDateRange" @on-change="dateChange" type="daterange" :placeholder="searchdetail.placeholder"></DatePicker>
        </FormItem>
    </span>
</template>

<script>
    import moment from 'moment';
    export default {
        name: 'searchitem',
        props: ['searchdetail'],
        data () {
            let dateEnd = moment();
            let dateStart = moment().subtract(1, 'months');
            console.log(dateStart.format('YYYY-MM-DD'));
            return {
                name: 'jingchenxu',
                defaultDateRange: [dateStart.format('YYYY-MM-DD'), dateEnd.format('YYYY-MM-DD')]
            };
        },
        mounted () {
            let me = this;
            // 将初始化好的数据传递过去
            if (me.$props.searchdetail.type === 'date') {
                let dateRange = [];
                dateRange[0] = moment(me.defaultDateRange[0]).format('YYYY-MM-DD');
                dateRange[1] = moment(me.defaultDateRange[1]).format('YYYY-MM-DD');
                this.$emit('searchDateChange', dateRange);
            }
        },
        methods: {
            valueChange (e) {
                this.$emit('paramsChange', this.$props.searchdetail.name, e.target.value);
            },
            valueSelect (val) {
                this.$emit('paramsChange', this.$props.searchdetail.name, val);
            },
            dateChange (val) {
                this.$emit('searchDateChange', val);
            }
        }
    };
</script>
```

可以看到，这里主要做的工作就是，将多个表单输入组件，汇总到一个组件当中，通过v-if进行条件判断，左右的组件对外的表现通过封装来保持一致，这里基本实现了动态组件的需求，但是如果在未来需要的表单输入组件的类型增多了之后，需要手动修改此组件才能实现组件功能的扩展。

## vue提供动态组件功能

说实在的vue提供的api实在是太多了，现在想来react提供的api就相对较少了，大量的功能需要引入第三发npm包来实现，所以可能学习路线比较的陡峭，好了，让我们说说vue的动态组件功能，该功能配合ES6的 import 及 对象自展开功能，可以完成一个灵活的动态组件。

首先我们还是来看下代码：

```javascript
<template>
  <Card :bordered="false" :dis-hover="true" class="searchbar-comtainer">
    <Form :model="searchbar" inline :label-width="80">
      <component @valueChange="valueChange" :itemConfig="item" :key="index" v-for="(item, index) in searchConfig.searchList" :is="item.component"></component>
      <FormItem>
        <Button v-if="searchConfig.canSearch" type="primary" @click="handleSearch" icon="ios-search">查询</Button>
        <Button v-if="searchConfig.canExport" type="primary" @click="handleExport" icon="ios-redo">导出</Button>
        <Button v-if="searchConfig.canDelete" type="primary" @click="handleDelete" icon="ios-close">删除</Button>
        <Button v-if="searchConfig.canAdd" type="primary" @click="handleAdd" icon="android-add">新增</Button>
      </FormItem>
    </Form>
  </Card>
</template>

<script>
import * as formcomponents from './formcomponents'
export default {
  name: 'searchbar',
  components: {...formcomponents},
  props: {
    searchConfig: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      searchbar: {
        user: '',
        password: ''
      },
      searchParams: {}
    }
  },
  methods: {
    handleSearch () {
      // eslint-disable-next-line
      let me = this;
      me.$emit('searchSubmit', me.searchbar);
    },
    handleExport () {
      // 导出
    },
    handleDelete () {
      // 删除
      let me = this;
      me.$emit('deleteSubmit');
    },
    handleAdd () {
      // 新增
    },
    valueChange (key, value) {
      this.searchParams[key] = value;
      console.dir(this.searchParams);
    }
  }
}
</script>
```

```javascript
<template>
  <FormItem :label-width="labelwidth" :label="itemConfig.label">
    <DatePicker @on-change="rangeChange" type="datetimerange" :placeholder="placeholder" style="width: 300px"></DatePicker>
  </FormItem>
</template>

<script>
export default {
  name: 'daterangeitem',
  props: {
    itemConfig: {
      type: Object,
      required: true
    }
  },
  computed: {
    placeholder () {
      return `请输入${this._props.itemConfig.label}`
    },
    labelwidth () {
      return this._props.itemConfig.labelwidth || 80
    }
  },
  data () {
    return {
      value: ''
    }
  },
  methods: {
    rangeChange (e) {
      console.dir(e);
      this.$emit('valueChange', this._props.itemConfig.prop, value)
    }
  }
}
</script>
```

所有的组件按照第二段代码来实现，当需要新增一个类型的组件时，只需要在formcomponents文件下创建，并引入到index.js当中即可，在不改变已有代码的情况下，实现了动态组件的扩展。

