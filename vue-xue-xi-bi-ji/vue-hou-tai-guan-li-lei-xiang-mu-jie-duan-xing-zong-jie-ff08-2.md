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

