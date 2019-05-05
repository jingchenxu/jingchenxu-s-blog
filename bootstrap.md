# bootstrap

在bootstrap的使用中modal是常使用的一个组件，如果是简单的弹出，使用非常的方便：

```markup
 data-toggle="modal" data-target="#myModal"
```

data-toggle：表示触发的组件类型；

data-target：表示触发的哪个该类型组件；

但是该组件有一个问题那就是在iOS的微信浏览器下使无效的，这个时候的补救办法是使用事件监听。

