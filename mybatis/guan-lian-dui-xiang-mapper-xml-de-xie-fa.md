##  关联对象mapper.xml的写法

>产生如题的需求的原因如下，在查询订单列表的时候，订单表中只有地址的编号，不存在地址的详细信息，这时候需要表关联查询，将查询的结果映射到javabean中，在订单的bean中，还存在一个地址的bean，如何将查询的结果映射到地址bean当中呢？

mybatis支持将数据库查询结果映射为一个resultmap，通过resultmap进一步的将查询结果与Javabean对应起来：

```xml
<resultMap id="cardDetailResultMap" type="com.lflweb.entity.mar.MarMem">
	    <result column="memid" property="memid" jdbcType="VARCHAR"/>
	    <result column="prdid" property="prdid" jdbcType="VARCHAR"/>
	    <result column="custid" property="custid" jdbcType="VARCHAR"/>
	    <result column="orderid" property="orderid" jdbcType="VARCHAR"/>
	    <result column="createdate" property="createdate" javaType="java.util.Date"/>
	    <result column="ondate" property="ondate" javaType="java.util.Date"/>
	    <result column="offdate" property="offdate" javaType="java.util.Date"/>
	    <result column="usedtimes" property="usedtimes" jdbcType="INTEGER"/>
	    <result column="resttimes" property="resttimes" jdbcType="INTEGER"/>
	    <result column="memtype" property="memtype" jdbcType="VARCHAR"/>
	    <result column="addressid" property="addressid" jdbcType="VARCHAR"/>
	    <result column="memstatus" property="memstatus" jdbcType="VARCHAR"/>
	    <result column="orimemid" property="orimemid" jdbcType="VARCHAR"/>
	    <result column="prdname" property="prdname" jdbcType="VARCHAR"/>
	    <result column="selectedid" property="selectedid" jdbcType="VARCHAR"/>
	    <association property="address" javaType="com.lflweb.entity.ctm.CtmAddress">
	        <id column="addressid" property="addressid" jdbcType="VARCHAR"/>
			<result column="addcontact" property="addcontact" jdbcType="VARCHAR"/>
			<result column="addmobile" property="addmobile" jdbcType="VARCHAR"/>
			<result column="contactsex" property="contactsex" jdbcType="VARCHAR"/>
			<result column="addprov" property="addprov" jdbcType="VARCHAR"/>
			<result column="addcity" property="addcity" jdbcType="VARCHAR"/>
			<result column="addcounty" property="addcounty" jdbcType="VARCHAR"/>
			<result column="adddetail" property="adddetail" jdbcType="VARCHAR"/>
			<result column="addtype" property="addtype" jdbcType="VARCHAR"/>
			<result column="isdefault" property="isdefault" jdbcType="BOOLEAN"/>
			<result column="addstatus" property="addstatus" jdbcType="VARCHAR"/>
			<result column="custid" property="custid" jdbcType="VARCHAR"/>
	    </association>
	</resultMap>
	
	<select id="GetCardListByCustId" resultMap="cardDetailResultMap" parameterType="com.lflweb.entity.mar.MarMem" statementType="CALLABLE">
		{call P_Get_CardListByCustId(
		    #{search.search,javaType=String,jdbcType=VARCHAR},
		    #{search.start,javaType=int,jdbcType=INTEGER},
		    #{search.end,javaType=int,jdbcType=INTEGER},
		    #{search.total,javaType=int,jdbcType=INTEGER,mode=OUT}
		)}
	</select>
```