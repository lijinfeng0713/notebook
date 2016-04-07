###### 20160406  

## Spring事务  
#### 1 基本概念  
&nbsp;&nbsp;&nbsp;&nbsp;事务就是一系列的动作，它们被看成是一个单独的工作单元，这些动作要么全部完成，要么全部都不起作用。事务管理用来保证数据的完整性和一致性。  
###### 事务的四个关键属性  
* 原子性  
* 一致性  
* 隔离性  
* 持久性    

#### 2 声明事务  
##### 2.1 基于注解的形式  
① 配置一个事务管理器  
② 启用事务注解  
③ 在对应的方法上添加@Transactional注解     
##### 2.2 基于配置的形式配置事务  
###### 步骤：  
① 配置事务管理器（transactionManager）  
② 配置事务属性   <tx:advice>      
③ 配置事务切入点，以及把事务切入点和事务属性关联起来    
```java  
  <!-- 配置事务管理器-->
  <bean id="transactionManager" class="org.springframework.orm.hibernate4.HibernateTransactionManager">
      <property name="sessionFactory" ref="sessionFactory" />
  </bean>
    
  <!-- 配置事务属性 -->
  <tx:advice id="transactionAdvice" transaction-manager="transactionManager">
		<tx:attributes>
	    <tx:method name="save*" propagation="REQUIRED" />
	    <tx:method name="add*" propagation="REQUIRED" />
	    <tx:method name="create*" propagation="REQUIRED" />
	    <tx:method name="insert*" propagation="REQUIRED" />
	    <tx:method name="update*" propagation="REQUIRED" />
	    <tx:method name="del*" propagation="REQUIRED" />
	    <tx:method name="remove*" propagation="REQUIRED" />
	    <tx:method name="put*" propagation="REQUIRED" />
	    <tx:method name="use*" propagation="REQUIRED"/>
	    <tx:method name="batchSave*" propagation="REQUIRED" />
	    <tx:method name="batchUpdate*" propagation="REQUIRED" />
	    <tx:method name="batchDelete*" propagation="REQUIRED" />
	    
	    <!--hibernate4必须配置为开启事务 否则 getCurrentSession()获取不到-->
	    <tx:method name="get*" propagation="REQUIRED" />
	    <tx:method name="query*" propagation="REQUIRED" />
	    <tx:method name="count*" propagation="REQUIRED" read-only="true" />
	    <tx:method name="find*" propagation="REQUIRED" read-only="true" />
	    <tx:method name="list*" propagation="REQUIRED" read-only="true" />
	    <tx:method name="*" read-only="true" />
		</tx:attributes>
	</tx:advice>
	
	<!-- 配置事务切入点以及把事务切入点和事务属性关联起来-->
	<aop:config expose-proxy="true">
    <!-- 只对业务逻辑层实施事务 -->
    <aop:pointcut id="transactionPointcut" expression="execution(* com.web.service.impl..*.*(..))" />
    <aop:advisor advice-ref="transactionAdvice" pointcut-ref="transactionPointcut" />
  </aop:config>  
```  

#### 3 事务的传播行为  
在注解中使用propagation指定事务的传播行为，即指定当当前事务方法被另外一个方法调用时如何使用事务。  
##### Spring支持的七种事务传播行为  
###### ① ` REQUIRED `  
使用调用方法的事务  
![](../img/2016040601.jpg)       
当执行过程中出现异常时，事务回滚到开始处，中间的所有方法都不会执行。
###### ② ` REQUIRED_NEW `  
表示该方法必须启动一个新的事务，并在自己的事务中进行，如果有事务在运行，就应该将这个事务挂起。  
```java  
	@Transactional(propagation = Propagation.REQUIRED_NEW)
	public void purchase() {...}
```  
![](../img/2016040602.jpg)    
如果在各自的事务中出现了异常，只回滚到自己事务的开始，不影响其他事务的执行  
###### ③ ` SUPPORTS `  
如果事务在运行，当前方法就在这个事务内运行，否则它可以不运行在事务中。  
###### ④ ` NOT_SUPPORTED `  
当前的方法不应该运行在事务中，如果有运行的事务，就将它挂起。  
###### ⑤ ` MANDATORY `  
当前方法必须运行在事务中，如果没有正在运行的事务，就抛出异常。  
###### ⑥ ` NEVER `  
该方法不能运行在事务中，如果当前事务正在运行，则抛出异常。  
###### ⑦ ` NESTED `   
如果当前存在事务，则在嵌套事务中执行。如果当前没有事务，则执行与REQUIRED类似的操作。  

#### 4 事务的其他属性  
* ` isolation ` : 指定事务的隔离级别，最常用的值为` READ_COMMITED `  
* ` readOnly ` : 指定事务是否是只读，表示当前事务只读取数据但不更新数据，这样也可以帮助数据库引擎优化事务   
* ` timeout ` : 指定强制回滚之前事务可占用的时间  
