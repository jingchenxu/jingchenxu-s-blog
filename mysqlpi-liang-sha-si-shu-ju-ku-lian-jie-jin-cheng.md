## MySQL批量杀死数据库连接进程

````sql
SELECT CONCAT('KILL ',id,';')  AS command    
FROM information_schema.processlist      
WHERE TIME>10
````

上面这段语句的作用是，查询所有的MySQL连接进程id,并将连接id拼接为进程杀死语句，复制查询接口直接执行即可。

具体见下图：

![bottom](/img/navicat/killprocess.gif)



