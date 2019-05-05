# LESS 简介

## 为什么用less

为什么不之间使用css，是啊为什么呢？他有以下的几点优势让我选择在日常的开发中使用它：

可以定义变量：这点在为了实现主题的时候很好用，将主题的颜色定义为变量，或是在很多地方会使用的相同高度，相同宽度等等，我们都可以定义为变量。

可以定义方法：就是mixins，可以提取样式的公共部分，而且还支持传递参数。

自带函数：less自带一些函数，这些函数在处理颜色或其他属性的值的时候非常方便。

## 怎么用less

less浏览器是解析不了的，怎么办呢？肯定是要编译成css的啊！less 只是让你写的爽了，所以写好的less文件需要编译成css然后再放到项目中使用。

如何将编写的less编译成css呢？

```javascript
// 全局安装less
npm install -g less
// 将css文件转化为cs文件
lessc style.less > styles.css
```

在webpack打包的项目中该如何使用么，那就需要我们在webpack的modules中配置相关的额loader来解析less文件：

```javascript
      {
        test: /\.less$/,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "less-loader",
        ]
      },
```

### 可定义变量

我们可以将一些需要重复使用的值设置为变量，可以是色值，可以是高度，接下来我会生命这一组不同类型的按钮的颜色，less代码如下：

```text
// 主题颜色
@primary-color:blue;
@info-color:green;
@warning-color:yellow;

.btn {

}
```

