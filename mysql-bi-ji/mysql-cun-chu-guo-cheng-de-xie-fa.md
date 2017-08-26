## MySQL存储过程的写法

- if else 语句

> 使用情景：当某条件成立时修改一条数据，否则插入一条数据

````sql
BEGIN
	#Routine body goes here...
  SELECT COUNT(*) INTO @count from t_ord_shop a WHERE a.custid=_custid AND a.prdid = _prdid;
  
  IF @count>0 THEN
     UPDATE t_ord_shop a SET
     prdcount = a.prdcount+1,
     updatedate = NOW()
     where a.custid=_custid AND a.prdid = _prdid;
  ELSE
     INSERT INTO t_ord_shop(custid,prdid,prdcount,createdate,shopstatus,updatedate)
			VALUES (_custid,_prdid,1,NOW(),'01',NOW());	
  END IF;

END
````

- 存储过程中的局部变量

> 计算查询结果的条数，作为局部变量用于下一步的判断或计算之类

````sql
BEGIN
  # 通过set进行赋值
  SET @testid = 'OR201705150002435080';
  # SELECT 也可以进行赋值，并且会在结果集中显示，赋值的默认值为NULL
  SELECT @result, @test :=23, @test1, @testid;
	SELECT COUNT(*) INTO @result from t_ord_base a WHERE a.orderid=@testid;

END
````

- switch case 语句

> 可将对于同一张表的不同类型的操作写在同一个存储过程当中，通过传入不同的参数来执行。

````sql
CREATE DEFINER = `skip-grants user`@`skip-grants host` PROCEDURE `NewProc`(IN `actiontype` varchar(2), IN `orderid` varchar(20))
BEGIN
	#Routine body goes here...
  SET @testid = 'OR201705150002435080';
  SET @testid0 = 'OR201705150014935141';
  SET @casetype = `actiontype`;
  CASE `actiontype`
       WHEN '01' THEN
       SELECT * FROM t_ord_base a WHERE a.orderid=@testid;

       WHEN '02' THEN
       SELECT * FROM t_ord_base a WHERE a.orderid=@testid0;
  END CASE;

END;
````
