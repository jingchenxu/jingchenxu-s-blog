# redux 调试工具使用

## 基本的概念

_Provider_

如果你的项目使用了react、react-redux，在使用react调试工具的时候，你是否会发现，在页面的最外层包裹了一个组件，这个组件使我们添加上去的，一般都是采用这种方式写的：

```jsx
<Provider store={store}>
        <div>
            <Router history={history} routes={routes}/>
        </div>
    </Provider>
```

> Provider 是由react-redux提供的一个组件，这个组件有一些可配置的属性，例如：store，这里的store即为redux中的state，则redux中的state（和react中组件的state不是一回事），这变为Provider的props，再通过Provider的props向下传递数据，这里也很好的践行了react的单向数据流的思想。 此外store中除了有数据，此外还有操作这些数据的方法的句柄，传递过来的数据用于页面渲染，而传递过来的修改数据的方法，则提供了页面与数据交互的方式，而react是基于数据进行渲染的，只要数据进行了修改，页面自然而然的就会随之发生修改，从而实现数据与页面的相互绑定。

Provider可以理解为一个react高阶组件，高阶组件通过组件外包裹，实现一个封装及mixin等一系列的功能，[rc-form](https://github.com/fis-components/rc-form)也是这一思想的践行者。

## 观察所有state

就我个人的理解，redux中的state并不是react组件中的state，我认为对于redux更好的理解应该是，redux是单页应用的数据仓库，这样的理解或许也不是十分的准确，在思索一下，我会认为redux应该是单页应用的数据模型，是一个及事件管理和数据集合于一身的数据模型，事件的管理用于操作模型。

## refrence

1. [解读redux工作原理](https://segmentfault.com/a/1190000004236064?utm_source=Weibo)

