## tomcat使用问题

### 设置tomcat的启动时长

tomcat在启动的时候，可能要获取较多的配置文件，如果设置的tomcat启动时长比较短的话，那么会造成一个问题，那就是，tomcat的启动时长会超过设置的配置时间，进而会出现以下的提示：

```
Server Tomcat v7.0 Server at localhost was unable to start within 45 seconds. If the server requires
```

造成出现该问题的原因可能是配置文件比较的大，又或者是当时的网络环境比较的差，这个时候需要我们手动来调节tomcat的启动时间。

具体操作如下，双击server，在显示的配置页面重新调整tomcat启动的时间限制

![zhelishitupain](/img/QQ图片20170116141635.png)

### （8005,8080,8009）端口占用

频繁的重启tomcat有时会导致出现这样的错误，面对这样的问题有3种解决办法：

1、重启电脑（网管式）<br/>
2、通过netstat -abn 找到端口究竟被那个进程占用，并干掉该进程 （学霸式）<br/>
3、在进程管理里面找到javaw 进程，直接干掉  （经验式）<br/>
4、其实在3的基础上可以判断，出现该问题的原因是tomcat崩溃的进程还占据着这些端口，我们可以通过运行tomcat的关闭脚本来强制关闭进程，如果你的开发环境是win，则在tomcat->bin->shutdown.bat,双击运行脚本关闭，如果是ubuntu的话，我觉得一个写代码如此有逼格的人，也不用我多说了。

