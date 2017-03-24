## input文件传输

### 原生的表单提交时怎样的

最近在项目中需要进行图片的提交，主要是通过input标签的 file类型进行提交，那么提交的数据究竟是什么，提交的时候是以什么样的数据格式提交的呢？

下面是一个简单的数据提交的示例：

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link href="css/style.css" rel="stylesheet">
</head>

<body>
    <input id="test" type="file" multiple name="" value=""><input id="button" type="button" value="提交">
</body>
<script>
    window.onload = function () {
        var input = document.getElementById('test');
        var button = document.getElementById('button');
        button.addEventListener('click', function () {
            console.log('show message!', input.files)
        })
    }

</script>

</html>
```

我们把提交的数据打印出来看看：

![](img/input/QQ截图20170324142951.png)

