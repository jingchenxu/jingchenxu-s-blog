## springboot 通过注解实现权限控制

- 让我们认识注解

springboot实现权限控制可以使用shiro,是的springboot可以帮我们完成很多的事情，其实这篇文章的重点是注解，但是单独的说注解没什么意思，每样技术，我更关心的是它能做什么，我们为什么需要它，那么注解是我们需要的么？我想，它确实是被需要的，因为在自己写项目的时候会用到很多的注解比如说@RequestMapping、@Override,所以我觉的我需要了解一下它。

- 注解是什么？

我们先不说注解具体的定义是什么？我们先打个比方，说一下注解像什么，我的理解啊！注解像个便利贴，@Override是个便利贴，这个便利贴，提醒我在编译的时候检查是不是实现的是接口中的方法，有了这个注解，这个类本身的活动并没有发生改变，这个类是个处理servlet的类，加了注解后他还是这个功能，没有变，但是他影响了别人，这个别人可以看做是编译这件事情，编译的时候编译器看到了这个注解。

- 为什么我们需要注解？

有没有发现注解和一个东西很像，xml配置文件，我们在使用springMVC这个框架的时候，常常会用到这样的一个注解@RequestMapping,这个组件有一个value属性，用于记录路由信息，如果我们不适用springMVC这样的框架，如何实现一个servlet呢？可以参考下[servlet 是什么](springmvcgong-zuo-ji-lu/servlet-shi-shi-yao.md)
，你会发现这里的这里的路由是通过XML来配置的，使用这种方式会有一些问题，比每次添加一个servlet都需要在XML文件当中配置一下，而如果使用注解，则只需要在controller处理方法的注解中添加一下，这里我们需要一个强耦合的配置方式。至于springMVC的@RequestMapping的实现方式可以参考下文档【7】。

- 实现基于注解的权限控制

创建权限注解

````java
package com.deepwater.daisy.annocation;

import java.lang.annotation.*;
// 该注解作用于方法
@Target(ElementType.METHOD)
// 该注解在代码运行时也是存在的
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Access {

    String[] value() default {};
    String[] authorities() default {};
    String[] roles() default {};
}
````

创建一个controller：

````java
    @RequestMapping(value = "/admin")
    // 通过注解配置该handler只能被拥有admin权限的人调用
    @Access(authorities = {"admin"})
    public String hello() {
        return "Hello, admin";
    }
````

创建一个拦截器，用于在调用controller之前判断权限信息：

````java
package com.deepwater.daisy.interceptor;

import com.deepwater.daisy.annocation.Access;
import org.springframework.util.StringUtils;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor ;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.*;
import java.lang.reflect.Method;
import java.util.HashSet;
import java.util.Set;

/**
 * @author jcxu
 * @date 20171012
 * @description 定义了一个拦截器
 */
public class UserInterceptor implements HandlerInterceptor  {


    public UserInterceptor() {
        super();
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("请求开始被执行！");
        // 将handler强转为HandlerMethod, 前面已经证实这个handler就是HandlerMethod
        HandlerMethod handlerMethod = (HandlerMethod) handler;
        // 从方法处理器中获取出要调用的方法
        Method method = handlerMethod.getMethod();
        // 获取出方法上的Access注解
        Access access = method.getAnnotation(Access.class);
        if (access == null) {
            // 如果注解为null, 说明不需要拦截, 直接放过
            return true;
        }
        if (access.authorities().length > 0) {
            // 如果权限配置不为空, 则取出配置值
            String[] authorities = access.authorities();
            Set<String> authSet = new HashSet<>();
            for (String authority : authorities) {
                // 将权限加入一个set集合中
                authSet.add(authority);
            }
            // 这里我为了方便是直接参数传入权限, 在实际操作中应该是从参数中获取用户Id
            // 到数据库权限表中查询用户拥有的权限集合, 与set集合中的权限进行对比完成权限校验
            String role = request.getParameter("role");
            if (!role.isEmpty()) {
                if (authSet.contains(role)) {
                    // 校验通过返回true, 否则拦截请求
                    return true;
                }
            }
        }
        // 拦截之后应该返回公共结果, 这里没做处理
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("请求正在被执行！");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("请求被执行完成！");
    }
}
````

注册该拦截器：

````java
package com.deepwater.daisy.configuration;

import com.deepwater.daisy.interceptor.UserInterceptor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

@Configuration
public class InterceptorConfig extends WebMvcConfigurerAdapter {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //这里会对"/admin"的请求进行拦截
        registry.addInterceptor(new UserInterceptor()).addPathPatterns("/admin");
    }
}
````

我们可以看到没有请求会交个一个handler来处理，这个handler是谁呢！他就是我们用@RequestMapping注解的方法啊！我们会通过反射，找到注解对象的属性，获取到的属性，就像是获取到了该handler的标签，我们知道了这些handler的权限信息，注解对于其作用的包、类、方法并没有产生影响，但是在拦截器中，通过判断注解信息，实现了不同的操作。

本篇暂未赘述注解的相关定义，可以通过相关文章了解相关的定义。

- 相关文章

[1][ Java 注解的作用与使用](http://blog.csdn.net/chenchaofuck1/article/details/52006961)
[2][Spring boot配置拦截器](http://blog.csdn.net/jiaobuchong/article/details/50394427)
[3][SpringBoot使用自定义注解实现权限拦截](http://www.jianshu.com/p/43c97352aa1e)
[4][理解Java基础之注解Annotation](http://developer.51cto.com/art/201107/276760.htm)
[5][java中自定义注解的作用和写法](http://m.blog.csdn.net/qq_36453032/article/details/53992961)
[6][Java 注解指导手册 – 终极向导](http://www.importnew.com/14227.html)
[7][JavaWeb学习总结(四十九)——简单模拟Sping MVC](http://www.cnblogs.com/xdp-gacl/p/4101727.html)