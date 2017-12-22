## Mybatis 如何获取存储过程返回值

- 不是方法的方法

所有的方法都是被问题催生的，我是不会蛋疼到这么做的，高并发如何处理线程安全是个蛋疼的问题，尤其在活动限购，问题上，如果不适用redis，问题还是有些蛋疼的，当然可以简单的使用线程安全的关键字，但是在体验上可能就会有点问题，而高并发的问题，主要就在与系统与数据库直接的交互问题上，应为系统与数据库的交互时很费时间的，而所有的请求都是异步的，这就为数据记录出错打下了基础。

- 存储过程是否线程安全

存储过程是否线程安全，我是不知道的，但是mysql是由行锁及表锁的，所以我想到了将业务逻辑交由存储过程来实现，好吧吐槽我吧？存储过程实现业务逻辑不是好办法，但我还是要干下去，因为存在行级锁，编不下去了

- 如何获取存储过程的返回值

在存储过程中会指定存储过程的参数，参数是IN 或是OUT,其中这里的IN表示的输入，而OUT表示的输出，如何在Mybatis中的xml mapper文件中配置呢？这里要涉及到select节点的resultType参数，这个参数只有select节点才有：

具体的我们可以看下设置：

````xml
	<parameterMap type="java.util.HashMap" id="testParameterMap">
		<parameter property="recordid" jdbcType="VARCHAR" mode="IN" />
		<parameter property="eventid" jdbcType="VARCHAR" mode="IN" />
		<parameter property="eventdate" jdbcType="VARCHAR" mode="IN" />
		<parameter property="prizeid" jdbcType="VARCHAR" mode="IN" />
		<parameter property="prizeall" jdbcType="INTEGER" mode="IN" />
		<parameter property="prizeget" jdbcType="INTEGER" mode="IN" />
		<parameter property="action" jdbcType="INTEGER" mode="IN" />
		<parameter property="out" jdbcType="VARCHAR" mode="OUT" />
	</parameterMap>

	<!-- 抽奖活动按日汇总表保存(t_eve_base_record) 业务逻辑交由存储过程执行 -->
	<select id="saveEveRecord" flushCache="true" parameterMap="testParameterMap" resultType="java.lang.String" statementType="CALLABLE">
		{call P_Save_JcxuEveBaseRecord(?,?,?,?,?,?,?,?)}
	</select>
````

- 我们看下通过dao层直接调用

````java
	@RequestMapping("/setrecord")
	@ResponseBody
	public ReturnValue setRecord() {
		ReturnValue rtv = new ReturnValue();
		
		Map<String, String> parmap = new HashMap<String, String>();
		parmap.put("recordid", "RE201712215558551558");
		parmap.put("eventid", "ET201712219551570873");
		parmap.put("eventdate", "20171219");
		parmap.put("prizeid", "EP201711149418968612");
		parmap.put("prizeall", "50");
		parmap.put("prizeget", "23");
		parmap.put("action", "3");
		parmap.put("out", "");
		
		eveDao.saveEveRecord(parmap);
		
		System.out.println(parmap.get("out"));
		return rtv;
	}
````

- 存储过程如下

````sql
CREATE DEFINER = `root`@`%` PROCEDURE `NewProc`(IN `_recordid` varchar(20),IN `_eventid` varchar(20),IN `_eventdate` varchar(8),IN `_prizeid` varchar(20),IN `_prizeall` int,IN `_prizeget` int,IN `_action` int,OUT `_out` varchar(20))
BEGIN
	IF _action = 2
THEN
		INSERT INTO t_eve_base_record(recordid, eventid, eventdate, prizeid, prizeall, prizeget)
			VALUES (_recordid, _eventid, _eventdate, _prizeid, _prizeall, _prizeget);

	ELSEIF _action = 3
	THEN
    # 获取活动的类型
    SELECT eventtype INTO @eventtype FROM t_eve_base WHERE eventid=_eventid;
    # 获取活动当前限制的总量
    SELECT prizeall INTO @prizeall FROM t_eve_base_record WHERE eventid=_eventid LIMIT 1;
    # 获取当天的

    CASE @eventtype
      WHEN '02' THEN
      # 计算已有的数量
      SELECT SUM(prizeget) INTO @prizeget FROM t_eve_base_record WHERE eventid=_eventid;
      IF(@prizeget<@prizeall) THEN
		    UPDATE t_eve_base_record SET 
			    eventid = eventid,
			    prizeid = prizeid,
			    prizeall = prizeall,
			    prizeget = prizeget+1,
          recordid = recordid
          where eventdate = _eventdate;
        SET _out='04';
      ELSE
        SET _out='05';
      END IF;
    END CASE;

	ELSEIF _action = 4
	THEN
		DELETE FROM t_eve_base_record WHERE recordid = _recordid;
	END IF;
END;

````