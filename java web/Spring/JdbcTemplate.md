###### 20160405  

## JdbcTemplate  
#### 1 概述  
JdbcTemplate是Spring里最基本的JDBC模板，利用JDBC和简单的索引参数查询提供对数据库的简单访问，从而可以简化JDBC代码。  
#### 2 JdbcTemplate的使用  
###### ① 数据库连接配置  
###### db.properties  
```java  
jdbc.driverClassName=com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://localhost:3306/shop
jdbc.username = root
jdbc.password = 123456
```  
###### ② 进行基本的配置  
* 导入数据资源文件  
* 配置数据源  
* 配置jdbcTemplate的bean  

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
	 
	<!-- 配置jdbcTemplate -->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"></property>	
	</bean>
```  

##### ③ Junit进行测试   
```java  
package com.fengge.jdbc;

import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import javax.sql.DataSource;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.springframework.jdbc.core.JdbcTemplate;
public class TestJdbcTemplate {

	private ApplicationContext ctx = null;
	private JdbcTemplate jdbcTemple;
	
	{
		ctx = new ClassPathXmlApplicationContext("config/application-context.xml");
		jdbcTemple = (JdbcTemplate) ctx.getBean("jdbcTemplate");
	}
	
	@Test
	public void testDataSource() throws SQLException {
		DataSource dataSource = ctx.getBean(DataSource.class);
		System.out.println(dataSource.getConnection());
	}
	
	@Test
	public void addUser() {
		String sql = "INSERT INTO user(username,password,email) VALUES (?,?,?)";
		jdbcTemple.update(sql,"Mam","123","123@qq.com");
	}
	
	@Test
	public void batchAdd() {
		String sql = "INSERT INTO user(username,password,email) VALUES (?,?,?)";
		List<Object[]> batchArgs = new ArrayList<>();
		batchArgs.add(new Object[]{"zhangxueyou","12","123@qq.com"});
		batchArgs.add(new Object[]{"liudehua","123","123@qq.com"});
		batchArgs.add(new Object[]{"zxc","1234","123@qq.com"});
	    jdbcTemple.batchUpdate(sql, batchArgs);
	}
	
	@Test
	public void updateUser() {
		String sql = "UPDATE user SET username = ? WHERE id = ?";
		jdbcTemple.update(sql, "Jack",93);
	}
	
	@Test
	public void delete() {
		String sql = "Delete From user WHERE username = ?";
		jdbcTemple.update(sql,"Mac");
	}
}
```    

#####  JdbcTemplate主要提供以下五类方法  
* ` execute `方法：可以用于执行任何SQL语句，一般用于执行DDL语句；  
* ` update `方法及` batchUpdate `方法：` update `方法用于执行新增、修改、删除等语句；` batchUpdate `方法用于执行批处理相关语句；   
* ` query `方法及` queryForXXX `方法：用于执行查询相关语句；  
* ` call `方法：用于执行存储过程、函数相关语句。  

##### JdbcTemplate支持的回调类  
###### （1）预编译语句及存储过程创建回调  
用于根据JdbcTemplate提供的连接创建相应的语句；  
###### ` PreparedStatementCreator `   
通过回调获取JdbcTemplate提供的Connection，由用户使用该Conncetion创建相关的PreparedStatement；  
###### ` CallableStatementCreator `    
通过回调获取JdbcTemplate提供的Connection，由用户使用该Conncetion创建相关的CallableStatement；  
###### (2) 预编译语句设值回调  
用于给预编译语句相应参数设值    
###### ` PreparedStatementSetter `   
通过回调获取JdbcTemplate提供的PreparedStatement，由用户来对相应的预编译语句相应参数设值；  
###### ` BatchPreparedStatementSetter `  
类似于PreparedStatementSetter，但用于批处理，需要指定批处理大小；    
###### (3) 自定义功能回调   
提供给用户一个扩展点，用户可以在指定类型的扩展点执行任何数量需要的操作  
###### ` ConnectionCallback `   
通过回调获取JdbcTemplate提供的Connection，用户可在该Connection执行任何数量的操作；  
###### ` StatementCallback `    
通过回调获取JdbcTemplate提供的Statement，用户可以在该Statement执行任何数量的操作；  
###### ` PreparedStatementCallback `    通过回调获取JdbcTemplate提供的PreparedStatement，用户可以在该PreparedStatement执行任何数量的操作；  
###### ` CallableStatementCallback `  通过回调获取JdbcTemplate提供的CallableStatement，用户可以在该CallableStatement执行任何数量的操作；   
###### (4) 结果集处理回调  
通过回调处理ResultSet或将ResultSet转换为需要的形式   
###### ` RowMapper `  
用于将结果集每行数据转换为需要的类型，用户需实现方法mapRow(ResultSet rs, int rowNum)来完成将每行数据转换为相应的类型。  
###### ` RowCallbackHandler `  
用于处理ResultSet的每一行结果，用户需实现方法processRow(ResultSet rs)来完成处理，在该回调方法中无需执行rs.next()，该操作由JdbcTemplate来执行，用户只需按行获取数据然后处理即可。    
###### ` ResultSetExtractor `  
用于结果集数据提取，用户需实现方法extractData(ResultSet rs)来处理结果集，用户必须处理整个结果集；  








