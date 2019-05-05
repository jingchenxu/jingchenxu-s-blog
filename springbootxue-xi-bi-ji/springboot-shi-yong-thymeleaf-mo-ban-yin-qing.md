# springboot 使用 thymeleaf 模板引擎

* 配置pom

在pom中配置一下代码：

```markup
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

* 配置application文件properties

```text
# 禁用thymeleaf缓存
spring.thymeleaf.cache=false
# 其他配置
spring.thymeleaf.prefix=classpath:/templates
spring.thymeleaf.suffix=.html  
spring.thymeleaf.mode=HTML5
spring.thymeleaf.content-type=text/html
```

* 添加一个简单的html模板

```markup
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>hello world</title>
  </head>
  <body>
    <div class="container">
      <div class="starter-template">
        <h1>Spring MVC / Thymeleaf / Bootstrap</h1>
        <p class="lead" th:text="${greeting}">(greeting)</p>
        <p>The current time is <span th:text="${currentTime}">(time)</span></p>
      </div>
    </div>
  </body>
</html>
```

* controller层添加访问路径

```java
package com.jingchenxu.springboot.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

@Controller
public class ThymeleafController {

    @RequestMapping("/hi")
    public String hello(Locale locale, Model model) {
        model.addAttribute("greeting", "Hello!");

        Date date = new Date();
        DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
        String formattedDate = dateFormat.format(date);
        model.addAttribute("currentTime", formattedDate);

        return "/hello";
    }
}
```

