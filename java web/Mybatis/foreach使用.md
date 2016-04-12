###### 20160412  

## Mybatis之foreach循环    
foreach是Mybatis中一个强大的动态SQL语句构造标签，他可以遍历一个数组，list或者是map，构造AND/OR或IN字句。  
##### foreach标签中常用的属性  
* ` item `：迭代元素的别名   
* ` collection `: 如果传入的参数类型是List，collection的值是list；如果传入的参数类型是数组，collection的值是array；如果传入的参数类型是map，collection的值是map的key    
* ` open `: 表示该语句以什么开始    
* ` separator `: 表示在每次进行迭代之间以什么符号作为分隔符   
* ` close `: 表示以什么结束       
 
##### 一、传入List  
```java  
<insert id="batchAdd" parameterType="List" useGeneratedKeys="true" keyProperty="id">
	INSERT INTO user (username,password,email) VALUES 
	<foreach item="u" collection="list" separator=",">
		(#{u.username}, #{u.password}, #{u.email})
	</foreach>
</insert> 
```   
###### java代码  
```java  
  @Test
	public void testBatchAdd() {
		List<User> list = new ArrayList<User>();
		User u1 = new User();
		u1.setUsername("AAA");
		u1.setPassword("AAA");
		u1.setEmail("AAA");
		list.add(u1);
		
		User u2 = new User();
		u2.setUsername("BBB");
		u2.setPassword("BBB");
		u2.setEmail("BBB");
		list.add(u2);
		
		userService.batchAdd(list);
	}
```   

##### 二、传入Map  
```java  
<delete id="batchDel">
	DELETE FROM user WHERE id IN 
	<foreach collection="ids" item="id" open="(" separator="," close=")">
		#{id}
	</foreach>
</delete> 
```    
###### java 代码    
```java  
	@Test
	public void testBatchDel() {
		List<Integer> list = new ArrayList<Integer>();
		list.add(121);
		list.add(122);
		list.add(123);
		list.add(124);
		Map<String, Object> param = new HashMap<String, Object>();
		param.put("ids", list);
		userService.batchDel(param);
	}
```   
注意：在传入map时，Collection的值为map的key，而且open，separator，close这些属性都不能省略。  
