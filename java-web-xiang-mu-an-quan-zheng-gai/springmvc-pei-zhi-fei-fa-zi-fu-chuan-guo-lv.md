---
description: 关于非法字符串的过滤使用过滤器进行过滤，过滤器需要在容器的上下文中进行配置，所以需要在项目web.xml文件中进行配置。
---

# SpringMVC 配置非法字符串过滤

## 新增过滤器

过滤器可以对所有的请求进行过滤，过滤器代码如下：

```java
public class IllegalCharacterFilter extends OncePerRequestFilter {  
  
    private static final String EVENTS = "(onload|onunload|onchange|onsubmit|onreset"  
            + "|onselect|onblur|onfocus|onkeydown|onkeypress|onkeyup|onerror"  
            + "|onclick|ondblclick|onmousedown|onmousemove|onmouseout|onmouseover|onmouseup)";  
    private static final String XSS_HTML_TAG = "\\n\\r|(%3C)|(%3E)|[<>]+";  
    private static final String XSS_INJECTION = "(%22|')|" + EVENTS + "|(%3D)|(%7C)";  
    private static final String XSS_REGEX = XSS_HTML_TAG + "|" + XSS_INJECTION;  
    private static final String SQL_REGEX = "(%27)|(')|(--)|(and)|(or)";  
  
    private static Pattern xssPattern = Pattern.compile(XSS_REGEX, Pattern.CASE_INSENSITIVE);  
    private static Pattern sqlPattern = Pattern.compile(SQL_REGEX, Pattern.CASE_INSENSITIVE);  
  
    public String filterDangerString(String value) {  
        if (value == null) {  
            return null;  
        }  
        //根据自己实际需求过滤  
        System.out.println(value);  
        value = xssPattern.matcher(value).replaceAll("");  
        value = sqlPattern.matcher(value).replaceAll("");  
        return value;  
    }  
  
    @Override  
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)  
            throws ServletException, IOException {  
        // TODO Auto-generated method stub  
        System.out.println("过滤器拦截到请求");  
        // TODO 此处用于处理非法字符串  
  
        filterChain.doFilter(new HttpServletRequestWrapper(request) {  
  
            @Override  
            public String getParameter(String name) {  
                // 返回值之前 先进行过滤  
                return filterDangerString(super.getParameter(name));  
            }  
  
            @Override  
            public String[] getParameterValues(String name) {  
                // 返回值之前 先进行过滤  
                String[] values = super.getParameterValues(name);  
                if (values != null) {  
                    for (int i = 0; i < values.length; i++) {  
                        values[i] = filterDangerString(values[i]);  
                    }  
                }  
                return values;  
            }  
        }, response);  
    }  
  
}  

```

## 注册过滤器

在项目的 WebContent -&gt; WEB-INF -&gt; web.xml 文件中中注册过滤器，配置代码如下：

```markup
<!-- 非法字符串过滤器 -->  
<filter>  
    <filter-name>illegalCharacterFilter</filter-name>  
    <filter-class>com.seed.filter.IllegalCharacterFilter</filter-class>  
</filter>  
<filter-mapping>  
    <filter-name>illegalCharacterFilter</filter-name>  
    <url-pattern>/*</url-pattern>  
</filter-mapping>  

```



