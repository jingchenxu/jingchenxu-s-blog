### import的威力

今天看到一个代码的写法，主要在于对import关键字的使用上，import除了导入正常的代码模块之外，还可以导入文件为变量使用：

示例代码如下：

```javascript
import SocialGithub from '../../images/GitHub-Mark-Light-120px-plus.png';
```

在使用的时候则可以直接通过下面的写法进行使用：

```javascript
<image style={styles.image} src={SocialGithub} />
```

