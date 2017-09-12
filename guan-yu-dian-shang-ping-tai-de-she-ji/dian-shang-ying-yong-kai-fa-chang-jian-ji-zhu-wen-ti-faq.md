## 电商应用开发产常见技术问题FAQ

> 本人有幸参与过三个电商应用的开发，在开发的过程中遇到较多的问题，有些问题是因为自身的技术能力不够，有些是因为自己的眼界不够造成的，其实电商应用进过了这么长时间的发展，已经有很多的坑被填过了，市面上有很多的成熟的解决方案，更多的时候，我们是要根据自己的业务特性来决定采用什么样的技术，下面是我在电商应用的开发中总结的一些常见技术问题。

1. 数据库设计

在项目的前期我们会使用诸如powerdesigner之类的数据库表设计工具来设计表，注释性的信息都会被写在表设计工具生成的文件当中，在项目组成员进行开发的时候，每个人都需要获取最新的表设计工具文件，如果我们在数据库中直接可以查看到相关的字表及表注释是不是更好，所以建议可以将字段及表的注释信息可以写在数据库当中，其实表结构设计工具同样也是提供相关的功能的，可参考下面的表生成SQL:

````sql
CREATE TABLE `t_log_client_error` (
  `logid` varchar(20) NOT NULL comment '日志编号',
  `message` VARCHAR(50) NULL comment '报错信息',
  `stack` text NULL comment '报错详情',
  `openid` VARCHAR(50) null comment 'openid',
  `logtime` VARCHAR(20) NULL comment '上报时间',
  `isread` BOOL NULL comment '是否已阅读',
  PRIMARY KEY (`logid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 comment '客户端监听日志表'
````

由于生产环境及测试环境直接的切换，我们有时候会需要将测试环境的表结构同步到生产环境，使用Navicat的数据同步工具，我们有时候会发现有些数据是无法同步过去的，这是因为某些表不存在主键，所以在设计表的时候我们需要设计主键。

