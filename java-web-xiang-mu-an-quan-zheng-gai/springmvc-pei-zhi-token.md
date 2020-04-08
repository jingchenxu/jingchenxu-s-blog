# SpringMVC 配置 token

## 后端实现

### 引入token生成依赖

在项目pom.xml中引入以下依赖

```markup
<dependency>  
   <groupId>com.auth0</groupId>  
   <artifactId>java-jwt</artifactId>  
   <version>3.8.3</version>  
</dependency> 

```

### Token生成工具类

编写token生成工具类，用具生成token、校验token是否正确、解密token中加密信息，具体代码实现如下：

```java
package com.seed.utils;  
  
import com.auth0.jwt.JWT;  
import com.auth0.jwt.JWTVerifier;  
import com.auth0.jwt.algorithms.Algorithm;  
import com.auth0.jwt.exceptions.JWTDecodeException;  
import com.auth0.jwt.interfaces.DecodedJWT;  
  
import java.util.Date;  
import java.util.HashMap;  
import java.util.Map;  
  
public class JwtUtils {  
    /** 
     * 过期时间一天， 正式运行时修改为30分钟 
     */  
    private static final long EXPIRE_TIME = 30 * 60 * 1000;  
    /** 
     * token私钥 
     */  
    private static final String TOKEN_SECRET = "f26e587c28064d0e855e72c0a6aad647";  
  
    /** 
     * 校验token是否正确 
     * 
     * @param token 密钥 
     * @return 是否正确 
     */  
    public static boolean verify(String token) {  
        try {  
            Algorithm algorithm = Algorithm.HMAC256(TOKEN_SECRET);  
            JWTVerifier verifier = JWT.require(algorithm).build();  
            DecodedJWT jwt = verifier.verify(token);  
            return true;  
        } catch (Exception exception) {  
            return false;  
        }  
    }  
  
    /** 
     * 获得token中的信息无需secret解密也能获得 
     * 
     * @return token中包含的用户名 
     */  
    public static String getUsername(String token) {  
        try {  
            DecodedJWT jwt = JWT.decode(token);  
            return jwt.getClaim("loginName").asString();  
        } catch (JWTDecodeException e) {  
            return null;  
        }  
    }  
  
    /** 
     * 获取登陆用户ID 
     *  
     * @param token 
     * @return 
     */  
    public static String getUserId(String token) {  
        try {  
            DecodedJWT jwt = JWT.decode(token);  
            return jwt.getClaim("userId").asString();  
        } catch (JWTDecodeException e) {  
            return null;  
        }  
    }  
  
    /** 
     * 生成签名,15min后过期 
     * 
     * @param username 用户名 
     * @return 加密的token 
     */  
    public static String sign(String username, String userId) {  
        // 过期时间  
        Date date = new Date(System.currentTimeMillis() + EXPIRE_TIME);  
        // 私钥及加密算法  
        Algorithm algorithm = Algorithm.HMAC256(TOKEN_SECRET);  
        // 设置头部信息  
        Map<String, Object> header = new HashMap<>(2);  
        header.put("typ", "JWT");  
        header.put("alg", "HS256");  
        // 附带username，userId信息，生成签名  
        return JWT.create().withHeader(header).withClaim("loginName", username).withClaim("userId", userId)  
                .withExpiresAt(date).sign(algorithm);  
    }  
}  


```

### 用户登录时需要生成Token

### 通过拦截器校验Token

```java
	package com.seed.interceptor;  
	  
	import com.seed.entity.ReturnValue;  
	import com.seed.entity.user.OnlineUser;  
	import com.seed.utils.Consts;  
	import com.seed.utils.JwtUtils;  
	  
	import org.springframework.web.method.HandlerMethod;  
	import org.springframework.web.servlet.HandlerInterceptor;  
	import org.springframework.web.servlet.ModelAndView;  
	  
	import javax.servlet.http.HttpServletRequest;  
	import javax.servlet.http.HttpServletResponse;  
	import java.io.PrintWriter;  
	import java.util.Arrays;  
	  
	public class TokenInterceptor implements HandlerInterceptor {  
	  
	    @Override  
	    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {  
	          
	        if(handler instanceof HandlerMethod) {  
	            HandlerMethod h = (HandlerMethod)handler;  
	            // 可以使用注解实现更加细化的权限控制  
	        } else {  
	              // 暂时用这种方法不对静态资源做拦截  
	            return true;  
	        }  
	          
	        response.setCharacterEncoding("utf-8");  
	        response.setContentType("application/json; charset=utf-8");  
	        ReturnValue rtv = new ReturnValue();  
	        String token = request.getHeader("token");  
	        OnlineUser onlineUser = (OnlineUser) request.getSession().getAttribute(Consts.DEFAULT_USER);  
	        System.out.println("TokenInterceptor"+request.getRequestURI());  
	          
	        // 部分接口无需token校验  
	        String[] noInterceptor = {"/smolab/user/login.do",  
	                "/smolab/user/logout.do",  
	                "/smolab/user/getonlineuser.do"};  
	        if (Arrays.asList(noInterceptor).contains(request.getRequestURI()))  
	            return true;  
	          
	        if (token == null) {  
	            rtv.setMsg("非法请求！");  
	            PrintWriter out = null;  
	            out = response.getWriter();  
	            out.append(rtv.toString());  
	              
	            return false;  
	        }  
	  
	        // 判断session中是否存在用户登录信息  
	        if (onlineUser != null) {  
	            // 校验token是否失效  
	            if (JwtUtils.verify(token)) {  
	                // 判断请求头中的token是否与session中的token一致  
	                if (onlineUser.getToken().equals(token)) {  
	                    return true;  
	                } else {  
	                    rtv.setMsg("token异常");  
	                }  
	            } else {  
	                // rtv.setMsg("需要刷新token");  
	                PrintWriter out = response.getWriter();  
	                out.write("login");  
	                  
	                return false;  
	            }  
	        } else {  
	            PrintWriter out = response.getWriter();  
	            out.write("login");  
	              
	            return false;  
	        }  
	          
	        // 返回token校验失败信息  
	        PrintWriter out = null;  
	        out = response.getWriter();  
	        out.append(rtv.toString());  
	          
	        return false;  
	    }  
	  
	    @Override  
	    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {  
	  
	    }  
	  
	    @Override  
	    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {  
	        // token快要失效时给它续费 如果已经超时 则返回  
	        // 请求完成后刷新token  
	        // OnlineUser onlineUser = (OnlineUser) request.getSession().getAttribute(Consts.DEFAULT_USER);  
	        // String token = JwtUtils.sign(onlineUser.getUser().getUsername(), onlineUser.getUser().getUserid());  
	        // onlineUser.setToken(token);  
	        // request.setAttribute(Consts.DEFAULT_USER, onlineUser);  
	        // 将新的token告诉客户端  
	        // response.addHeader("token", token);  
    }  
}  

```

## 前端实现

### 前端获取token

前端只有在登录时才能获取token，用户登录时token将会与登录信息一同返回给前台，目前的做法是将token存储在vuex当中。

### 在请求头中添加token

因为后端对所有的请求都会添加拦截，所以前端所有的请求头中都需要添加token身份信息，前端ajax请求使用axios，在axios中使用拦截器，获取vuex 中的token信息添加到请求头当中，具体实现代码如下：

```javascript
	request.interceptors.request.use(req => {  
	  // TODO 在请求头中添加token 可以将token缓存到cookie当中 判断到没有token时就需要回到登录页  
	  let token = store.state.user.currentUser.token;  
	  req.headers.common['token'] = token;  
	  // 对get请求方法自动添加时间戳避免请求到缓存  
	  const METHOD = req.method;  
	  if (METHOD === 'get') {  
	    switch (Object.prototype.toString.call(req.params)) {  
	      case '[object URLSearchParams]':  
        // 用于修复请求在IE中被缓存的问题  
        req.params.append('timestamp', new Date().getTime());  
        break;  
      case '[object Object]':  
        req.params['timestamp'] = new Date().getTime();  
        break;  
      case '[object Undefined]':  
        req['params'] = {  
          timestamp: new Date().getTime()  
        };  
        break;  
      default:  
        throw new Error('无匹配项');  
    }  
  }  
  return req;  
}, error => {  
  return Promise.reject(error.error);  
}); 

```



