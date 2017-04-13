## VUE CLI 踩坑指南

### vue中的组件循环遍历

```javascript
<div class="good-item" v-for="good in goods.list1">
        
      </div>
```

### 图片动态url

```javascript
<img style="width: 100px;height: 140px;float: left;" :src="'http://51nestlewaters.com/nestle/'+market.goods.list1[index].PrdImage.imageurl"></img>  
```