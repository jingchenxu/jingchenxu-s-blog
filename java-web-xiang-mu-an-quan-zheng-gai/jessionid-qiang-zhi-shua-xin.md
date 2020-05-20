---
description: 在使用AppScan之类的安全扫描工具扫描时，可能会扫描到未更新的会话标识这样的问题。
---

# jessionid强制刷新

## 为什么要强制刷新？

因为不强制刷新会很危险，你的会话标识在被别人利用，你没登录的时候没关系，一旦你登录了，别人可以拿着你的会话标识访问相关数据，所以会话标识要刷新啊！

## 如何强制刷新jessionid?

后端添加filter，在 smobis&gt;src&gt;main&gt;java&gt;com&gt;seed&gt;filter 下添加 NewSessionFilter.java 文件，文件代码如下：

```text
package com.seed.filter;

import java.io.IOException;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Map.Entry;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.springframework.web.filter.OncePerRequestFilter;

public class NewSessionFilter extends OncePerRequestFilter {
    
    @Override
    public void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws IOException, ServletException {
    	
    	
    	if (request instanceof HttpServletRequest) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            if (httpRequest.getSession() != null) {
                System.out.println("old Session:" + httpRequest.getSession().getId());
                HttpSession session = httpRequest.getSession();
                HashMap<String, Object> old = new HashMap<String, Object>();
                Enumeration<String> keys = session.getAttributeNames();
                while (keys.hasMoreElements()) {
                    String key = (String) keys.nextElement();
                    old.put(key, session.getAttribute(key));
                    session.removeAttribute(key);
                }
                
                if (!httpRequest.getSession().isNew()){
                    session.invalidate();
                    session = httpRequest.getSession(true);
                    System.out.println("new Session:" + session.getId());
                }
 
                for (Iterator<Entry<String, Object>> it = old.entrySet().iterator(); it.hasNext();) {
                    Map.Entry<String, Object> entry = (Map.Entry<String, Object>) it.next();
                    session.setAttribute((String) entry.getKey(), entry.getValue());
                }
            }
        }
    	
        filterChain.doFilter(request, response);
        
    }
    
    @Override
    public void destroy() {
 
    }
    
    public NewSessionFilter() {
        System.out.println("NewSessionFilter");
    }
}
```

添加完成后需要注册filter。

