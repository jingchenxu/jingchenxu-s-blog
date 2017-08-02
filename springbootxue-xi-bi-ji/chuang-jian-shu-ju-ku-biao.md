## 数据库表创建

1.创建表 user 用户信息表 

```sql
DROP TABLE IF EXISTS  `user`;
CREATE TABLE `user` (
  `userID` int(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '用户编号',
  `userAge` int(10) unsigned  NOT NULL COMMENT '用户年龄',
  `userName` varchar(25) DEFAULT NULL COMMENT '用户姓名',
  `userAvator` varchar(100) DEFAULT NULL COMMENT '用户头像',
  PRIMARY KEY (`userID`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT '用户基础信息表';
```

> 对于表中自动增长的主键应该使用整型；创建表的时候将备注信息写上。


