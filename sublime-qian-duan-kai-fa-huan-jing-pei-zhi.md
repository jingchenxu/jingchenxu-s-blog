# sublime前端开发环境配置

* 首先下载sublime text3[下载地址](http://www.sublimetext.com/)；
* 下载完成后进行包管理插件的安装安装；

```python
import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```

* 如果不习惯英文的话，可以找一个中文的插件安装一下；

## 推荐插件

> Emmet[学习地址](http://blog.wpjam.com/m/emmet-grammar/)

