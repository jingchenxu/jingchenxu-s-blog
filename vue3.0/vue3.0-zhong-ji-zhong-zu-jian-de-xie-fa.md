---
description: 有点类似茴香豆的几种写法
---

# Vue3.0中几种组件的写法

## 组件的单文件编写方式

```javascript
<template>
  <div class="inner-container">
    <el-form :inline="true" :model="searchParams">
      <el-form-item label="用户编号">
        <el-input
          clearable
          v-model="searchParams.userid"
          placeholder="请输入用户编号"
          @keyup.enter.native="onSearch"
        ></el-input>
      </el-form-item>
      <el-form-item label="登录描述">
        <el-input
          clearable
          v-model="searchParams.logindesc"
          placeholder="请输入登录描述"
          @keyup.enter.native="onSearch"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          icon="el-icon-search"
          :loading="isSearching"
          type="primary"
          @click="onSearch"
          >查询</el-button
        >
        <el-button
          icon="el-icon-download"
          :loading="isSearching"
          type="primary"
          @click="onExport"
          >导出</el-button
        >
      </el-form-item>
    </el-form>
    <el-table
      :data="tableData"
      :height="tableHeight"
      ref="tableRef"
      v-loading="isSearching"
      border
      stripe
      size="mini"
      style="width: 100%"
    >
      <el-table-column v-if="showIndex" type="index" align="center">
      </el-table-column>
      <el-table-column
        v-for="column in tableColumns"
        :key="column.colCode"
        :prop="column.colCode"
        :label="column.colName"
        :width="column.colWidth"
        :formatter="headerFormat"
        :sortable="column.order"
        show-overflow-tooltip
      >
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="pageSizes"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="totalNumber"
    >
    </el-pagination>
  </div>
</template>

<script>
import http from '@/apis/index.js'
import _ from 'lodash'
import { onMounted, reactive, ref, computed, nextTick } from 'vue'

const name = 'LoginLog'

export default {
  name,
  setup() {
    let searchParams = reactive({})
    let sqlId = ref('LOGINLOG')
    let searchUrl = ref('search_login_log')
    let currentPage = ref(1)
    let showIndex = ref(true)
    let tableHeight = ref(500)
    let pageSize = ref(20)
    let pageArray = reactive([1, 2, 3, 4])
    let tableColumns = reactive([])
    let tableData = reactive([])
    let totalNumber = ref(0)
    let isSearching = ref(false)
    let isExporting = ref(false)

    const tableRef = ref(null)

    let pageSizes = computed(() => pageArray.map(page => page * 10))

    let countListTableHeight = () => {
      nextTick(() => {
        tableHeight.value =
          document.body.clientHeight - 50 - 44 - tableRef.value.$el.offsetTop
      })
      window.onresize = () => {
        nextTick(() => {
          tableHeight.value =
            document.body.clientHeight - 50 - 44 - tableRef.value.$el.offsetTop
        })
      }
    }

    let onSearch = () => {
      isSearching.value = true
      let params = new URLSearchParams()
      params.append('pagenumber', currentPage.value)
      params.append('pagesize', pageSize.value)
      let searchParams = _.cloneDeep(searchParams)
      for (let key in searchParams) {
        // TODO 有些参数需要处理下
        params.append(key, searchParams[key])
      }
      http
        .get(searchUrl.value, params)
        .then(res => {
          res.list.forEach(item => {
            tableData.push(item)
          })
          totalNumber.value = res.total
        })
        .finally(() => {
          isSearching.value = false
          countListTableHeight()
        })
    }

    let onExport = () => {
      console.log('点击了导出按钮')
    }

    let onLoadColumn = () => {
      let params = new URLSearchParams()
      params.append('sqlId', sqlId.value)
      http.get('get_column_by_sqlid', params).then(res => {
        res.data.forEach(item => {
          tableColumns.push(item)
        })
      })
    }

    let headerFormat = (row, column, cellValue) => {
      if (_.isBoolean(cellValue)) {
        return cellValue ? '是' : '否'
      }
      return cellValue
    }

    let handleSizeChange = pageSize => {
      pageSize = pageSize
      onSearch()
    }

    let handleCurrentChange = pageNumber => {
      currentPage = pageNumber
      onSearch()
    }

    onMounted(() => {
      onLoadColumn()
      onSearch()
      nextTick(countListTableHeight)
    })

    return {
      searchParams,
      sqlId,
      searchUrl,
      currentPage,
      showIndex,
      tableHeight,
      pageSize,
      pageArray,
      tableColumns,
      tableData,
      totalNumber,
      isSearching,
      isExporting,
      pageSizes,
      onSearch,
      onExport,
      headerFormat,
      handleSizeChange,
      handleCurrentChange,
      tableRef
    }
  }
}
</script>

```

{% hint style="info" %}
单文件的组件编写方式是兼容vue2.0的写法的，在option配置式的写法的基础上，新增了setup选项，在setup函数中可以通过函数式的方式组织代码的业务逻辑，本质上setup中的函数是需要进行编译的组件配置，在setup函数中不能够像以前一样直接过通过`this` 访问到组件实例。

所有的响应式数据需要通过`vue` 提供的函数进行包装，对于基础数据类型的包装，需要注意获取数据的值，需要通过 `.value` 的方式获取到包装对象对应的值。
{% endhint %}

[Vue3.0中如何使用jsx](https://github.com/vuejs/jsx-next/blob/dev/packages/babel-plugin-jsx/README-zh_CN.md)

