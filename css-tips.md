## css Tips

### 图片后文字与图片上下垂直居中

![bottom](/img/css-tips/bottom.png)

上图中可以看到在图片左侧的文字没有上下居中，或许我们可以将span转化为块级元素，通过float来实现上下居中，但是此刻有一个更好的办法，
vertical-align,图片后的文字如需上下垂直居中，在图片标签中需设置vertical-align:middle,这个时候，后面的文字需要相对图片不同的位置，则需要设置文字的vertical-align: bottom。

![bottom](/img/css-tips/active.gif)



