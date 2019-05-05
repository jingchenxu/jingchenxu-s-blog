# 响应式布局

## 一大推的meta标签介绍

```markup
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

允许网站宽度自动调整，这里的viewport是网页默认的宽度和高度，在网页中添加以上标签，则声明了网页的宽度默认是等于设备屏幕的的宽度的（width=device-width）,\(initial-scale=1\)表示原始的缩放比例为1.0。

## 尽量少使用绝对宽度

响应式布局会根据设备屏幕的宽度而不断的进行调整，故而最好式减少绝对宽度或高度的使用，最好是使用百分比或是rem\(rem:表示的是默认字体大小的倍数\)

## 适当使用display && visibility

可能某些模块在不同的尺寸下显示的方式发生了很大的改变，我们完全可以通过设置元素的显隐性来控制部分模块的显示和隐藏，其中display 设置为null的时候，整个元素是不可见，且不占位的，如果设置的是visibility 为 hidden，虽然不可见，但是其是占位的。

