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