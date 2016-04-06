###### 20160406  

## NameParameterJdbcTemplate   
#### 1 基本概念  
与JdbcTemplate一样，NameParameterJdbcTemplate也是Spring对JDBC的一种封装。在经典的 JDBC 用法中, SQL 参数是用占位符 ? 表示,并且受到位置的限制. 定位参数的问题在于, 一旦参数的顺序发生变化, 就必须改变参数绑定。这显然是不方便的，因此在NameParameterJdbcTemplate中提供了具名参数的用法。  
###### 具名参数  
SQL 按名称(以冒号开头)而不是按位置进行指定. 具名参数更易于维护, 也提升了可读性. 具名参数由框架类在运行时用占位符取代  

#### 2 基本用法   
##### ① 数据库连接配置  
###### db.properties  
```java  
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/shop
jdbc.username = root
jdbc.password = 123456
```  
##### ② 进行基本的配置  
* 导入数据资源文件  
* 配置数据源  
* 配置namedParameterJdbcTemplate的bean  

```
	<!-- 导入资源文件 -->
	<context:property-placeholder location="classpath:config/db.properties"/>
     
	<!-- 配置dbcp数据源 -->
	<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="${jdbc.driverClassName}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<property name="initialSize" value="1" />
	</bean>
	 
	<!--  配置nameParameterJdbcTemplate  -->
	<bean id="namedParameterJdbcTemplate"
    class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate">
    <constructor-arg ref="dataSource"></constructor-arg>
  </bean>
```  
##### ③ 使用namedParameterJdbcTemplate  
* 首先是要加载配置文件并获取bean。如下：    

```java  
	private ApplicationContext ctx = null;
	private NamedParameterJdbcTemplate namParameterJdbcTemplate;
	
	{
		ctx = new ClassPathXmlApplicationContext("config/application-context.xml");
		namParameterJdbcTemplate = ctx.getBean(NamedParameterJdbcTemplate.class);
	}
```  
* 使用` update(sql,paramMap) ` 方法    
```java  
	@Test
	public void addUser() {
		String sql = "INSERT INTO user(username,password,email) VALUES (:name,:email,:psw)";
		Map<String,String> paramMap = new HashMap<>();
		paramMap.put("name", "Chen");
		paramMap.put("psw", "123");
		paramMap.put("email", "123@qq.com");
		namParameterJdbcTemplate.update(sql, paramMap);
	}
```  
###### 注意：  
使用这种方法可以给参数起别命名（别名前必须加:）来代替传统的占位符，这样就不需要关注参数的顺序。然后再通过别名注入值，但这种方法很明显是比较麻烦的。  

* 使用` update(sql, paramSource) `方法  
```java 
	@Test
	public void insertUser() {
		String sql = "INSERT INTO user(username,password,email) VALUES (:username,:password,:email)";
		User u = new User();
		u.setUsername("Xijinping");
		u.setPassword("1234");
		u.setEmail("qwe@qq.com");
		SqlParameterSource paramSource = new BeanPropertySqlParameterSource(u);
		namParameterJdbcTemplate.update(sql, paramSource);
	}
```   
###### User.java  
```java  
public class User {

	private Integer id;
	private String username;
	private String password;
	private String email;
	
	//getter and setter

	@Override
	public String toString() {
		return id + username + password + email;
	}
}
```  
###### 注意： 
使用这种方法时，括号中的参数名必须跟JavaBean中的属性名保持严格一致，不然就会出错。  


