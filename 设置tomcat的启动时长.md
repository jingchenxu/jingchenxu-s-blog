## 设置tomcat的启动时长

tomcat在启动的时候，可能要获取较多的配置文件，如果设置的tomcat启动时长比较短的话，那么会造成一个问题，那就是，tomcat的启动时长会超过设置的配置时间，进而会出现以下的提示：

```
Server Tomcat v7.0 Server at localhost was unable to start within 45 seconds. If the server requires
```

造成出现该问题的原因可能是配置文件比较的大，又或者是当时的网络环境比较的差，这个时候需要我们手动来调节tomcat的启动时间。

具体操作如下，双击server，在显示的配置页面重新调整tomcat启动的时间限制

