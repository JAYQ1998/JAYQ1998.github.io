# 动态标签用法

> MyBatis 中@Select 注解使用动态标签时要用<script>括起来

## 1.if（重要）

> If : 当参数满足条件才会执行某个条件

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu WHERE age = 20 
 		 <if test="name != null">
    			AND name like #{name}
 	 	</if>
</select>
```

## 2.choose、when、otherwise

> choose标签是按顺序判断其内部when标签中的test条件是否成立，
>
> 如果有一个成立，则choose结束；
>
> 如果所有的when条件都不满足时，则执行otherwise中的SQL。
>
> 类似于java的switch语句。

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu WHERE age = #{age} 
	<choose>
 			 <when test="name != null">
    				AND name like #{name}
			</when>
			<when test="class != null">
					AND class like #{class}
			</when>
			<otherwise>
					AND class = 1
			</otherwise>
 	 </choose>
</select>
```



## 3.where

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu WHERE 
		<if test="age != null">
			age = #{age}
		</if> 
 		<if test="name!= null">
			AND name= #{name}
		</if> 
		<if test="class!= null">
			AND class = #{class}
		</if> 
</select>
```




```sql
-- 当第一个if不满
SELECT stu.name FROM tab_stu stu WHERE AND name = "小米" AND class ="1班”;
-- 当第一第二第三个if都不满足，会出现以下情况
SELECT stu.name FROM tab_stu stu WHERE;
```


这会导致查询失败。使用where标签可以解决这个问题

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu 
		<where> 
			<if test="age != null">
				age = #{age}
			</if> 
		 <if test="name!= null">
				AND name= #{name}
		</if> 
		<if test="class!= null">
				AND class = #{class}
		</if> 
	</where>
</select>
```

> where标签会在只有一个以上的if条件满足的情况下才去插入WHERE关键字，
>
> 而且，若最后的内容是”AND”或”OR”开头的，where也会根据语法绝对是否需要保留。

## 4.set

- set标签用于解决动态更新语句存在的符号问题

```sql
<update id="updateStu">
		Update tab_stu
		<set>
				<if test="name != null"> name=#{name},</if>
				<if test="age != null"> age=#{age},</if>
				<if test="class != null"> class=#{class},</if>
				<if test="subject != null"> subject=#{subject}</if>
		</set>
</update>
```

> set标签会动态前置SET关键字，同时也会消除无关的逗号，因为用了条件语句后，可能就会在生成的赋值语句的后面留下逗号。

## 5.trim

> trim标签可实现where/set标签的功能

- Trim标签有4个属性，分别为prefix、suffix、prefixOverrides、suffixOverrides
  - prefix：表示在trim标签包裹的SQL前添加指定内容
  - suffix：表示在trim标签包裹的SQL末尾添加指定内容
  - prefixOverrides：表示去掉(覆盖)trim标签包裹的SQL指定首部内容，去掉多个内容写法为and |or(中间空格不能省略)（一般用于if判断时去掉多余的AND |OR）
  - suffixOverrides：表示去掉(覆盖)trim标签包裹的SQL指定尾部内容（一般用于update语句if判断时去掉多余的逗号）

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu 
		<trim prefix="where" prefixOverrides="and |or">
				<if test="age != null">
						age = #{age}
				</if> 
 				<if test="name!= null">
						AND name= #{name}
				</if> 
				<if test="class!= null">
						OR class = #{class}
				</if> 
		</trim>
</select>
```



```sql
<update id="updateStu">
			Update tab_stu
			<trim prefix="set" subfix="where id=#{id}" suffixOverrides=",">
				<if test="name != null"> name=#{name},</if>
				<if test="age != null"> age=#{age},</if>
				<if test="class != null"> class=#{class},</if>
				<if test="subject != null"> subject=#{subject}</if>
			</trim>
</update>
```



## 6.foreach(重要)

> 对集合进行遍历

```sql
<select id="findName" resultType="String">
 		SELECT stu.name FROM tab_stu stu where id in
		<foreach item=”item” index=”index” collection=”listName” open=”(” separator=”,” close=”)”>
				#{item}
		</foreach>
</select>
```

下面是foreach标签的各个属性：

- collection：迭代集合的名称，可以使用@Param注解指定，该参数为必选(java入参，相对于#{listName})
  item：表示本次迭代获取的元素，若collection为List、Set或数组，则表示其中元素；若collection为Map，则代表key-value的value，该参数为必选
- index：在List、Set和数组中，index表示当前迭代的位置，在Map中，index指元素的key，该参数是可选项
  open：表示该语句以什么开始，最常使用的是左括弧”(”，MyBatis会将该字符拼接到foreach标签包裹的SQL语句之前，并且只拼接一次，该参数是可选项
- close：表示该语句以什么结束，最常使用的是右括弧”）”，MyBatis会将该字符拼接到foreach标签包裹的SQL语句末尾，该参数是可选项
- separator：MyBatis会在每次迭代后给SQL语句添加上separator属性指定的字符，该参数是可选项

## 7.bind

> bind标签可以从OGNL(对象图导航语言)表达式中创建一个变量并将其绑定到上下文
>
> Mybatis中使用Mysql的模糊查询字符串拼接（like） 中也涉及到bind的使用

```sql
<select id="findName" resultType="String">
 		 SELECT stu.name FROM tab_stu stu 
		<where>
 				<if test="name!= null">
 						<bind name="stuName" value="'%'+stuName+'%'">
						name like #{stuName}
				</if> 
		</where>
</select>
```

