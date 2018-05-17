# vue-cli项目集成pdfjs进行pdf文件流预览

> 这只是一份关于如何在vue-cli项目中集成pdf文件预览功能的说明，防止那天忘了，通过这篇记录可以少浪费点时。

### 集成准备

| pdfjs下载地址 | [https://github.com/mozilla/pdf.js/releases](https://github.com/mozilla/pdf.js/releases) |
| :--- | :--- |


release下载完成后，进行解压，将解压的文件全部放到vue-cli项目的static目录下，此时我们就可以看下效果了，我这里webpack-dev-server的端口号为8080，所以对应的地址为：http://localhost:8080/static/web/viewer.html，打开此地址，我们可以看到一个默认的pdf文件被打开，如果你需要显示一个指定地址的pdf文件怎么办呢，在这个页面的地址上添加一个参数即可http://localhost:8080/static/web/viewer.html?file=http://localhost:8080/static/web/demo.pdf。

如果你的文件是通过文件流接口提供的怎么办呢？我们可以看到这里的文件都是通过远程文件地址访问的，如果我们要把文件流显示出来该怎么办呢？

这里我们需要修改2个文件，分别为刚才解压文件中的viewer.js 和 viewer.html,在viewer.js文件中我们需要删除以下代码：

```js
var DEFAULT_URL = 'compressed.tracemonkey-pldi-09.pdf';
```

在viewer.html文件中需要添加上这段代码，我们将通过ajax请求获取pdf文件的二进制流并赋值给pdfjs插件，具体的代码如下：

```js
<script src="../build/pdf.js"></script>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
<script>
  var DEFAULT_URL = ""; //注意，删除的变量在这里重新定义  
var PDFData = "";
// 获取url当中出现的参数
function GetQueryString(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if (r != null)
        return unescape(r[2]);
    return "";
}
var url = GetQueryString('url') || '/smopark/pub/downattach.do?file.attachuuid=08676a6f6bb347d78469a88c88f5e39e';
$.ajax({
	type: "get",
	async: false, //
	mimeType: 'text/plain; charset=x-user-defined',
  url: url,
	success: function (data) {
		PDFData = data;
	}
});
var rawLength = PDFData.length;
//转换成pdf.js能直接解析的Uint8Array类型,见pdf.js-4068  
var array = new Uint8Array(new ArrayBuffer(rawLength));
for (i = 0; i < rawLength; i++) {
	array[i] = PDFData.charCodeAt(i) & 0xff;
}
DEFAULT_URL = array;
</script>

    <script src="viewer.js"></script>
```

可以看到上面的这段代码是放在viewer.js之前的，我们要确认DEFAULT\_URL有值。这里我们需要通过传递不同的参数而实现展示不同的pdf文件，所以我添加了一个参数名为url的请求参数，来获取二进制流的请求地址，最后我们可以通过这样的地址展示：http://localhost:8080/static/web/viewer.html?url=%2fsmopark%2fpub%2fdownattach.do%3ffile.attachuuid%3d08676a6f6bb347d78469a88c88f5e39e 可以看到url参数的值进行了UrlEncode。

