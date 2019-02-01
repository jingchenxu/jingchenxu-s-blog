# Java springMVC 项目开发总结

- 取一个好名字

编程语言也是语言，这门语言可以让我们和机器交流，但其实更多的时候我们的代码还会被别人看，如何让别人看懂自己的代码也是写代码时需要注意的，在阅读一些Java框架的源码的时候，会发现很多Java源码中的方法或变量的名称都比较的长，这是因为这些名称都是实际要表达的意思的单词的全拼，写代码时更像时在说话，尤其时一些业务代码，取一个好的名字会让代码的可读性提高哼多。

取一个好名字从表结构设计开始，表的字段名称就要取好，在我司的后台管理项目中，从前端到后端，分别时一个vue页面文件，vue-router路由，controller文件，service文件，dao文件，存储过程文件，PDM设计文件，一条龙，看见没有，所有的文件的名命方式都是相同的，bug的追踪会省很多的问题，前后端的对接也会方便很多。

不要过度封装，代码写久了，会不自觉的对自己的代码进行封装，分装之后的代码屏蔽了一些基础的代码，会让人觉得不是特别好理解。

异常处理，我其实一致对Java项目中的异常处理方式不是特别的了解，很多时候代码中都没有异常处理的机制，写了一堆的System.out.println()这样的代码，这样的代码在生产环境时对系统性能有所损耗的，最理想的状是在发生异常是输出日志，所以我们的日志输出需要写在catch当中，需要能够将当时代码中的变量值都进行输出，一边在发生异常后及时的还原当时的数据。

Javabean的设计，Javabean很重要，它会帮助我们解决很多的事情，目前我从日常开发中总结出来的一个Javabean可以使用IDE自动生成，首先可以创建一个基础的类，该类实现了序列化接口：

````java
package com.deepwater.base.entity;

import javax.persistence.*;
import java.io.Serializable;

public class BaseEntity implements Serializable {
    @Id
    @Column(name = "Id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Integer id;

    @Transient
    private Integer pagenumber = 1;

    @Transient
    private Integer pagesize = 10;

    // 查询条件字符串
    private String searchstr = null;

    // 查询开始日期
    private String begindatestr = null;

    // 查询结束日期
    private String enddatestr = null;

    // 表头查询参数
    private String sqlid = null;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public Integer getPagenumber() {
        return pagenumber;
    }

    public void setPagenumber(Integer pagenumber) {
        this.pagenumber = pagenumber;
    }

    public Integer getPagesize() {
        return pagesize;
    }

    public void setPagesize(Integer pagesize) {
        this.pagesize = pagesize;
    }

    public String getSearchstr() {
        return searchstr;
    }

    public void setSearchstr(String searchstr) {
        System.out.println(searchstr);
        this.searchstr = searchstr;
    }

    public String getBegindatestr() {
        return begindatestr;
    }

    public void setBegindatestr(String begindatestr) {
        this.begindatestr = begindatestr;
    }

    public String getEnddatestr() {
        return enddatestr;
    }

    public void setEnddatestr(String enddatestr) {
        this.enddatestr = enddatestr;
    }

    public String getSqlid() {
        return sqlid;
    }

    public void setSqlid(String sqlid) {
        this.sqlid = sqlid;
    }
}

````

为什么要实现序列化，因为负载均衡，mybatis开启缓存之类的功能，都需要Javabean实现序列化接口，所以要实现，一个通常的Javabean通常就是实现builder模式，重写toString方法，重写toString是为了日志输出方便，builder模式可以少些一些代码。


