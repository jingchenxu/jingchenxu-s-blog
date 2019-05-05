# 原生JavaScript  DOM操作

> 很多时候在操作dom的时候，都会默认的想到使用jQuery，如果我们不在借助这些第三方库，想要使用原声的JavaScript来操作DOM 我们需要怎么办，本着这样的想法尝试开始通过原声JavaScript 来操作DOM，突然发现其实也很方便很简单。

* 为什么需要使用原声JavaScript

不是所有的代码都需要建立在jQuery这类的一个框架上的，这些框架或者说是库，确实减少了很多的工作量，但是如果要使用，确实还是会需要引入大量的额外的代码，不值得，引入一个框架或是库可能对现有的代码带来哪些潜在的风险我们无法预知，或许使用原声代码会成为一种对原有代码侵入性比较低的方式。

* 原生产JavaScript添加modal弹出框

在一个已经开发好的商城的订单详情页，为活动添加一个分享提示框。

对于这样的需求，我打算将实现这些业务的代码写在一个js文件当中，原来的订单详情页只需要引入这段代码就可以了，保证对原有代码最小的入侵不需要额外引入多余的js。

可能是用习惯的原因，chrome确实是个不错的工具，当然Firefox也是不错的，但是chrome的移动端模拟器更好看，还带外壳，这个成为我最中意的一点，接下来我们直接在chrome的调试工具Console当中直接进行调试，体验确实不错。

```javascript
      var modalMask = document.createElement('div')
      modalMask.style.position = 'fixed'
      modalMask.style.zIndex = 1000
      modalMask.style.top = 0
      modalMask.style.right = 0
      modalMask.style.bottom = 0
      modalMask.style.left = 0
      modalMask.style.backgroundColor = 'rgba(0, 0, 0, 0.3)'
      var modalContent = document.createElement('img')
      modalContent.src = '/water/share/img/guide.png'
      modalContent.style.position = 'fixed'
      modalContent.style.top = '0px'
      modalContent.style.right = '0px'
      modalContent.style.zIndex = 9000
      var modal = document.createElement('div')
      modal.append(modalMask)
      modal.append(modalContent)
      modal.id = 'share-modal'
      var body = document.getElementsByTagName('body')[0]
      body.append(modal)
      modalMask.addEventListener('click', function () {
        body.removeChild(document.getElementById('share-modal'))
      })
```

上面的这段代码可以直接在浏览器中运行，这段原生代码实现了modal的效果，完全没有利用到额外框架的代码。

在以上代码的编写过程中，我不禁思考虚拟DOM的实现方式，在以上为DOM添加样式的过程中，我们发现了，样式的属性都是驼峰式的，这与react及VUE当中动态添加style的方式都是相同的，所谓的虚拟DOM树是否就是一个个的DOM对象，内存中操作对象确实是比页面中操作DOM方便的多。

