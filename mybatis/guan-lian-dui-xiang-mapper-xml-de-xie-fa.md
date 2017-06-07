##  关联对象mapper.xml的写法

>产生如题的需求的原因如下，在查询订单列表的时候，订单表中只有地址的编号，不存在地址的详细信息，这时候需要表关联查询，将查询的结果映射到javabean中，在订单的bean中，还存在一个地址的bean，如何将查询的结果映射到地址bean当中呢？

mybatis支持将数据库查询结果映射为一个resultmap，通过resultmap进一步的将查询结果与Javabean对应起来：

```xml
<!--这边的id与下面的对应，数据的类型可以是Javabean-->
<resultMap id="cardDetailResultMap" type="com.lflweb.entity.mar.MarMem">
            <!--这边是将获取的结果与Javabean一一对应-->
	    <result column="memid" property="memid" jdbcType="VARCHAR"/>
	    <result column="prdid" property="prdid" jdbcType="VARCHAR"/>
	    <result column="custid" property="custid" jdbcType="VARCHAR"/>
	    <result column="orderid" property="orderid" jdbcType="VARCHAR"/>
	    <!--对于javabean中存在子级Javabean的需要通过一下方法进行映射-->
	    <association property="address" javaType="com.lflweb.entity.ctm.CtmAddress">
	        <id column="addressid" property="addressid" jdbcType="VARCHAR"/>
			<result column="addcontact" property="addcontact" jdbcType="VARCHAR"/>
			<result column="addmobile" property="addmobile" jdbcType="VARCHAR"/>
			<result column="contactsex" property="contactsex" jdbcType="VARCHAR"/>
			<result column="addprov" property="addprov" jdbcType="VARCHAR"/>
	    </association>
	</resultMap>
	<!--这边是一个正常的调用数据库的过程，可以是通过SQL、存储过程，只需将输出的结果resultType改为与resultmap的映射-->
	<select id="GetCardListByCustId" resultMap="cardDetailResultMap" parameterType="com.lflweb.entity.mar.MarMem" statementType="CALLABLE">
		{call P_Get_CardListByCustId(
		    #{search.search,javaType=String,jdbcType=VARCHAR},
		    #{search.start,javaType=int,jdbcType=INTEGER},
		    #{search.end,javaType=int,jdbcType=INTEGER},
		    #{search.total,javaType=int,jdbcType=INTEGER,mode=OUT}
		)}
	</select>
```

在某些情况下如果出现一对多的情况要怎么办呢？实际的情况比如说是一对多的情况，这个时候就要用到

```xml
	<resultMap id="conDetailResultMap" type="com.lflweb.entity.mar.MarMemConDetail">
	    <result column="conid" property="conid" jdbcType="VARCHAR"/>
	    <result column="productiondate" property="productiondate" javaType="java.util.Date"/>
	    <result column="memid" property="memid" jdbcType="VARCHAR"/>
	    <result column="deliverstatus" property="deliverstatus" jdbcType="VARCHAR"/>
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
	    <collection property="memconList" select="GetMemDetails" column="conid" ofType="com.lflweb.entity.mar.MarMemDetail">
	    </collection>
	</resultMap>
	
	<resultMap id="conDetailsResultMap" type="com.lflweb.entity.mar.MarMemDetail">
	    <result column="traceid" property="traceid"/>
	    <result column="skuid" property="skuid"/>
	    <result column="prdspec" property="prdspec"/>
	    <result column="prdunit" property="prdunit"/>
	    <result column="count" property="count"/>
	    <result column="skuname" property="skuname"/>
	</resultMap>
	
	<!-- 宅配卡配送信息查询(t_mar_mem_Detail) -->
	<select id="GetMemDetail" resultMap="conDetailResultMap" statementType="CALLABLE" parameterType="com.lflweb.entity.mar.MarMemConDetail">
		{call
		P_Get_XjcMarConDetail(
		#{search.search,javaType=String,jdbcType=VARCHAR}
		)}
	</select>
	
	<!-- 宅配卡配送信息查询(t_mar_mem_Detail) -->
	<select id="GetMemDetails" resultMap="conDetailsResultMap" parameterType="String">
		select a.*,b.* from t_mar_mem_detail a LEFT JOIN t_prd_sku b on a.skuid=b.skuid where traceid = #{conid}
	</select>
```