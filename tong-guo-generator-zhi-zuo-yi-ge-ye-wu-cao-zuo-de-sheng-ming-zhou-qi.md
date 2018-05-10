# 通过generator制作一个业务操作生命周期

### 前言

> 所有的诞生都有其已存的使命，关于ES6的标准已经大致看过一遍，看的是网上的阮一峰的教程，看完了，但是印象不是特别的深刻，因为有些特性我不知道可以用来做什么。
>
> 在最近的一次开发中，大部分的前端页面的逻辑是类似的，框架上使用的是vue,所以我们将大量的页面上的通用逻辑写在mixin当中，比如最常见的列表页面的数据加载业务，在mixin的mounted的生命周期函数中默认执行我们的列表数据获取方法getDataList,列表数据获取时对服务端的一次数据查询。
>
> 列表的数据查询，我们会涉及到的业务主要有，条件查询，查询的条件需要在执行数据查询前准备好，那么问题来了，查询的条件可能是通过接口获取的，这时候我们就需要先执行查询条件接口的执行，在执行数据查询接口的执行。

### v1.0

1.0的版本我想的很简单，完全没有考虑到查询条件为异步的情况，只是简单在getDataList方法中的异步请求前添加了2个钩子函数，分别为getDataListBefore、getDataListAfter，具体的代码如下：

```js
    getDataList () {
      // 默认的获取列表页数据方法
      let me = this;
      if (me.getDataListBefore) {
        me.getDataListBefore();
      }
      let searchParams = this.searchParams;
      searchParams['search.start'] = (me.currentPage - 1) * me.pageSize + 1;
      searchParams['search.page'] = me.currentPage;
      searchParams['search.limit'] = me.pageSize - 1;
      searchParams = utils.addPreParams(searchParams, me.preParams)
      me.loading = true;
      this.$axios.get(`${this.searchUrl}`, {
        params: searchParams
      }).then(function (res) {
        let data = res.data;
        let tempList = [];
        for (let i = 0; i < data.length; i++) {
          let temp = data[i];
          temp['_index'] = i;
          tempList.push(temp);
        }
        me.data = tempList;
        me.total = res.total;
        me.loading = false;
        if (me.getDataListAfter) {
          // 在列表数据获取完成后触发的钩子函数
          me.getDataListAfter(res.data, res.total)
        }
      })
    },
```

这段代码的问题在于，只有getDataListBefore方法中的代码是同步代码才可以达到我们的效果，如果在getDataListBefore发起异步请求，在getDataList中可能会无法获取到想要的请求参数。

### v2.0

在2.0版本当中开始着手解决此问题，其实第一个想到的是使用promise来解决此问题，但是最后还是决定使用generator，原因以后再说，这里需要注意的是，generator通过执行next\(\)方法进入到函数的下一个状态，但是next\(\)方法只能在异步代码中执行，如果实在同步代码中执行，则会导致[`Generator is already running`](https://oss.so/article/82)问题，具体的实现代码如下：

```js
    getDataList () {
      // 通过generator使得getDataList的执行顺序按照getDataListBefore、_getDataList、getDataListAfter执行
      // 初始化generater
      let me = this
      function * dataGenerator () {
        me.getDataListBefore()
        if (me._getDataListBefore) {
          yield me._getDataListBefore()
        }
        yield me._getDataList()
        me.getDataListAfter()
        if (me._getDataListAfter) {
          yield me._getDataListAfter()
        }
        return 'ending'
      }
      me.datag = dataGenerator()
      me.datag.next()
    },
    getDataListBefore () {
      console.log('before======start======》')
      console.log('before======end======》')
    },
    // _getDataListBefore () {
    //   let me = this;
    //   console.log('_before======start======》')
    //   me.$axios.get(`${me.pmtUrl}`, {
    //     params: {
    //       name: 'userstatus'
    //     }
    //   }).then(function (res) {
    //     console.log('_before======end======》')
    //     me.datag.next()
    //   })
    // },
    _getDataList () {
      let me = this;
      console.log('pending======start======》')
      me.$axios.get(`${me.pmtUrl}`, {
        params: {
          name: 'userstatus'
        }
      }).then(function (res) {
        console.log('pending======end======》')
        me.datag.next()
      })
    },
    getDataListAfter () {
      console.log('after======start======》')
      console.log('after======end======》')
    },
    // _getDataListAfter () {
    //   let me = this;
    //   console.log('_after======start======》')
    //   me.$axios.get(`${me.pmtUrl}`, {
    //     params: {
    //       name: 'userstatus'
    //     }
    //   }).then(function (res) {
    //     console.log('_after======end======》')
    //     me.datag.next()
    //   })
    // }
```

其实上面的代码创建了一个列表数据获取的生命周期，主要分为3个部分，获取数据前，获取数据中，获取数据后，查询参数的准备放在获取参数前函数当中

