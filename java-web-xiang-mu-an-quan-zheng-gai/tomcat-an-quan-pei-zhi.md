---
description: 目前很多的安全扫描工具都会对应用环境进行扫描，因为很多的漏洞可能并不是你的代码的漏洞，而是你系统运行环境的漏洞。
---

# Tomcat 安全配置

## 修改tomcat默认管理页面地址

* 如果服务通过nginx转发，可以通过配置nginx将指向根目录的请求转发至指定目录；
* 如果用户直接访问 tomcat 服务器则需屏蔽 tomcat 管理页面地址： 修改tomcat默认跳转地址，在tomcat-》 conf 当中修改server.xml文件，在Host标签节点下添加配置如下：

```markup
<Host name="localhost"  appBase="webapps"  
  unpackWARs="true" autoDeploy="true">  
  
  <!-- SingleSignOn valve, share authentication between web applications  
       Documentation at: /docs/config/valve.html -->  
  <!--  
  <Valve className="org.apache.catalina.authenticator.SingleSignOn" />  
  -->  
 <Context path="" docBase="${catalina.home}/webapps/demo/" debug="0" reloadable="true"/>  
  
  <Value className="org.apache.catalina.valves.RemoteAddrValve" allow="127.0.0.1"/>  
  <!-- Access log processes all example.  
       Documentation at: /docs/config/valve.html  
       Note: The pattern used is equivalent to using pattern="common" -->  
  <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"  
         prefix="localhost_access_log" suffix=".txt"  
         pattern="%h %l %u %t "%r" %s %b" />  
  
</Host>  
```

在上面的代码可以看到，需要在tomcat-》webapps下配置demo 文件夹，并在此文件夹下新建 index.html 文件，index.html 文件内容如下：

```markup
<!DOCTYPE html>  
<html lang="en">  
<head>  
  <meta charset="UTF-8">  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">  
  <meta http-equiv="X-UA-Compatible" content="ie=edge">  
  <title>Tomcat</title>  
</head>  
<body>  
    
</body>  
<script>  
  window.onload = function() {  
    location.href = 'smolab'  
  }  
</script>  
</html>

```

## 修改Tomcat关闭不安全的http方法

进入tomcat-》 conf 目录当中编辑web.xml文件，在web.xml 文件尾部添加如下内容：

```markup
<security-constraint>    
        <web-resource-collection>    
            <url-pattern>/*</url-pattern>    
            <http-method>PUT</http-method>    

```

## 修改Tomcat避免泄露服务器版本信息

* 进入tomcat-》lib 目录当中找到catalina.jar 文件；
* 使用本机的压缩软件打开（注意不是解压）catalina.jar 文件，进入 org\apache\catalina\util 文件夹，找到ServerInfo.properties 文件，编辑文件中相关tomcat 版本信息（在最后三行），改为如下图所示：

![&#x4FEE;&#x6539;&#x540E;&#x7684;&#x670D;&#x52A1;&#x5668;&#x4FE1;&#x606F;](../.gitbook/assets/image.png)



* 修改完成后进行保存，关闭压缩软件，软件自动提示是否保存修改后的文件，点击确认则自动保存修改后的文件至 catalina.jar 文件当中，使用修改后的catalina.jar 文件替换tomcat 中的 catalina.jar 文件。

## 删除Tomcat下的非必须目录

删除

