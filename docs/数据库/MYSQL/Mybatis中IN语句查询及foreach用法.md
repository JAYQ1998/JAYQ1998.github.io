# 1 需求

> 查询 用户 ID 为 101、102、103 的数据，参数是一个集合

# 2 在 SQL 语句中

```sql
select * from t_user where user_id in ( '101' , '102' ,'103')
```



# 3 在 Mybatis 中

```xml
<select id="selectUserByIdList" resultMap="usesInfo">
	SELECT
	*
	from t_user
	WHERE id IN
	<foreach collection="idList" item="id" index="index" open="(" close=")" separator=",">
	  #{id}
	</foreach>
</select>
```



- 对应的 Mapper中的接口如下

> List<UserInfo> selectUserByIdList(@Param("idList")List idList)

# 4 扩展

foreach元素的属性主要有collection，item，index，open，separator，close。

## collection

用来标记将要做foreach的对象，作为入参时

- List对象默认用"list"代替作为键
- 数组对象有"array"代替作为键，
- Map对象没有默认的键。
- 入参时可以使用@Param(“keyName”)来设置键，设置keyName后，默认的list、array将会失效。
  如下的查询也可以写成如下

```
List<UserInfo> selectUserByIdList(List idList)
```




```
<select id="selectUserByIdList" resultMap="usesInfo">
	SELECT
	*
	from t_user
	WHERE id IN
	<foreach collection="list" item="id" index="index" open="(" close=")" separator=",">
	  #{id}
	</foreach>
</select>
```


如果是数组类型

```
List<UserInfo> selectUserByIdList(Long[] idList)
```

```
<select id="selectUserByIdList" resultMap="usesInfo">
	SELECT
	*
	from t_user
	WHERE id IN
	<foreach collection="array" item="id" index="index" open="(" close=")" separator=",">
	  #{id}
	</foreach>
</select>
```

## item

> 集合中元素迭代时的别名，该参数为必选。

## index

> 在list和数组中,index是元素的序号，在map中，index是元素的key，该参数可选

## open

> foreach代码的开始符号，一般是(和close=")"合用。常用在in(),values()时。该参数可选

## separator

> 元素之间的分隔符，例如在in()的时候，separator=","会自动在元素中间用“,“隔开，避免手动输入逗号导致sql错误，如in(1,2,)这样。该参数可选。

## close

> foreach代码的关闭符号，一般是)和open="("合用。常用在in(),values()时。该参数可选。