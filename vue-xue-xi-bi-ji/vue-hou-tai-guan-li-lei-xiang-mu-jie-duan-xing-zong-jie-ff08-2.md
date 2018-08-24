# vue 后台管理类项目阶段性总结（2）

### 如何处理iview组件中的bug

iview在日常的试用过程中体验还是非常好的，但是随着项目复杂度的提升，在iview的使用过程中确实遇到了一些bug,尤其Tab组件的bug比较多。遇到这样的问题我们怎么办呢？找一个替代品还是是自己写一个，这两种方案都不是我喜欢的，我比较讨厌一个项目里各种依赖，喜欢将项目的依赖尽量控制的比较少，没什么原因，就是喜欢。

自己写一个，其实也算是不错的方案，但是由于自身能力的显示，对于tabs这样稍显复杂的组件来说，自己写还是有一定的风险的，因为我不能保证自己写的比iview中的bug还少。

那么有没有一种方案，通过修改iview的源码来修复这个bug呢？可以是可以的，可以自己打包下载iview的源码，修复bug后，本地打包，在项目中引入自己打包的文件，这种方法存在这样的缺点，每次iview的升级十分的不方面，操作繁琐，那么又没由一种方法能够单独修改出现bug的组件呢？

最后在实践中终于想到了一个方法，通过extends API来重写出现bug的组件，然后通过全局注册，覆盖原先iview中对此组件的注册。

下面我们修复Tabs组件的问题：

```javascript
<script>
/* eslint-disable */
import {Tabs} from 'iview'

const transitionTime = 300; // from CSS

export default {
  name: 'Tabs',
  extends: Tabs,
  methods: {
    updateVisibility (index) {
      [...this.$refs.panes.children].forEach((el, i) => {
        if (index === i) {
          setTimeout(() => {
            [...el.children].forEach(child => child.style.visibility = 'visible');
          }, transitionTime)
          if (this.captureFocus) setTimeout(() => focusFirst(el, el), transitionTime);
        } else {
          setTimeout(() => {
            [...el.children].forEach(child => child.style.visibility = 'hidden');
          }, transitionTime)
        }
      })
    }
  }
}
</script>
```

然后我们在项目的入口文件当中重新全局注册此组件，用来覆盖出现问题的组件：

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import VueContentPlaceholders from 'vue-content-placeholders'
import App from './App'
import {router} from './router'
import vueplugin from './utils/vueplugin'
import 'iview/dist/styles/iview.css'
import iView from 'iview'
import Tabs from './components/iviewbugfix/Tabs'
import Progress from './components/iviewbugfix/Progress'
import Transfer from './components/iviewbugfix/Transfer'
import userselect from './components/userselect'
import store from './store'
// 在需要浏览器兼容性时再启用
// import 'babel-polyfill'
// import 'url-search-params-polyfill'
// import 'promise-polyfill'
// 对iview的组件进行了重写

Vue.config.productionTip = false;

Vue.use(vueplugin);
Vue.use(iView);
Vue.use(VueContentPlaceholders)

// 全局组件注册
// https://github.com/iview/iview/issues/4120 通过全局覆盖注册的方式临时解决此问题，后期可以通过版本升级解决此问题
Vue.component('Tabs', Tabs)
Vue.component('Progress', Progress)
// Vue.component('Transfer', Transfer)
Vue.component('UserSelect', userselect)

/* eslint-disable no-new */

var vm = new Vue({
  el: '#app',
  router,
  store,
  components: {
    App,
  },
  template: '<App/>'
});

```
通过这样的方法，我们可以临时处理iview组件库中出现的问题，如果iview组件库在后续的版本中修复了此问题，那我们只需要将这些代码删除，并升级iview的版本就可以了。

### 如何减少前端数据请求

在表单页面比较多的时候，我们会遇到较多的参数，这些参数都是通过接口获取的，而不是前端写死的，如果一个页面上的下拉参数比较多，那么发起的请求就会比较多，如何尽可能的减少这些请求呢？发起以此请求时可以接受的，但是无法接受的时每次加载页面的时候都会请求这些参数，因为这些参数表大部分情况下是不会变的。

所以需要将请求的结果缓存起来，只要用户不刷新浏览器，那么只要成功请求以此结果就会被缓存。

```javascript
/**
 * @author jcxu
 * @description 为表头参数表添加了缓存，默认不使用浏览器缓存，刷新页面时缓存丢失
 */

import axios from 'axios'

// 数据存储
export const cache = {
  data: {},
  set (key, data, bol = false) {
    if (bol) {
      localStorage.setItem(key, JSON.stringify(data))
    } else {
      this.data[key] = data
    }
  },
  get (key, bol = false) {
    if (bol) {
      return JSON.parse(localStorage.getItem(key))
    } else {
      return this.data[key]
    }
  },
  clear (key, bol = false) {
    if (bol) {
      localStorage.removeItem(key)
    } else {
      delete this.data[key]
    }
  }
}

// 建立唯一的key值
function buildUrl (url, params = {}) {
  const sortedParams = Object.keys(params).sort().reduce((result, key) => {
    result[key] = params[key]
    return result
  }, {})

  url += `?${JSON.stringify(sortedParams)}`
  return url
}

// 缓存,建议只给get加缓存
export default (options = {}) => config => {
  const { url, method, params, data } = config
  const { local = false } = options
  // 建立索引
  let index
  if (method === 'get') {
    index = buildUrl(url, params)
  } else {
    index = buildUrl(url, data)
  }
  const indexData = index + '-data'
  let response = cache.get(indexData, local)
  let responsePromise = cache.get(index)
  if (response) {
    return Promise.resolve(JSON.parse(JSON.stringify(response))) // 对象是引用，为了防止污染数据源
  } else if (!responsePromise) {
    responsePromise = (async () => {
      try {
        const response = await axios.defaults.adapter(config)
        cache.set(indexData, response, local)
        return Promise.resolve(JSON.parse(JSON.stringify(response))) // 同时发送多次一样的请求，没办法防止污染数据源，只有业务中去实现
      } catch (reason) {
        cache.clear(index, local)
        cache.clear(indexData)
        return Promise.reject(reason)
      }
    })()

    // put the promise for the non-transformed response into cache as a placeholder
    cache.set(index, responsePromise)
  }
  return responsePromise
}

```
请求时需要如下设置：

```javascript
    _getPmt (name) {
      // 通用的参数请求方法
      let me = this;
      if (!(me[name] && me[name].length > 0)) {
        me.$axios.get(`${me.pmtUrl}`, {
          params: {
            id: name
          },
          adapter: cache({
            local: false
          })
        }).then(function (res) {
          let all = [{
            id: '',
            name: '全部'
          }]
          if (me[name]) {
            me[name] = all.concat(res)
          } else {
            me.$set(me, name, all.concat(res))
          }
        })
      }
    },
```

