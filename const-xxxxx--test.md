### ES6解构赋值

今日在学习别人的代码时，看到了以下的一段代码：

```javascript
import { AppBar, IconButton, Styles } from 'material-ui';
```
一开始我是不能理解，但是在看到这种语法的中文名字的时候，我就明白了，结构赋值，获取``material-ui``对象中的键值对，并与前面的键名对应起来，并完成对应的赋值。

但是理解好像还是有点偏差的，因为我看到了一下的写法：

```javascript
// CommonJS模块
let { stat, exists, readFile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat, exists = _fs.exists, readfile = _fs.readfile;
```
结构赋值提供了更为便捷的从对象取值的方式；