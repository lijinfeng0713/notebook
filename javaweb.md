# Java Web 学习笔记
===================
### 1 EL表达式
#### 1.1 EL表达式基本语法
##### 1.1.1 立即执行
######  语法格式 ` ${expression } `
&nbsp;&nbsp;&nbsp;&nbsp;立即执行，EL表达式将在页面渲染的时候被JSP引擎解析和执行  

##### 1.1.2 延迟执行
######  语法格式 ` #{expression } `

### 2 使用核心标签库（C命名空间）
#### 2.1 使用方法
&nbsp;&nbsp;&nbsp;&nbsp;首先需要导入jstl.jar和standard.jar两个jar包，以及c.tld文件。其次，需要在jsp文件中加入taglib指令，如下：  
	` <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> `  
#### 2.2 常用标签
##### 2.2.1 ` <c:out> ` 标签
* 该标签的作用是帮助在jsp中输出内容
* 该标签默认如同fn:escapeXml 一样，对保留的xml字符进行了转义，这样处理可以保护网站避免遭到跨网站脚本攻击和各种注入攻击。但也可以自己手动禁用这种行为。禁用方法：` <c:out value="${someVariable}" escapeXml="false" /> `  

##### 2.2.2 ` <c:if> ` 标签  
用于控制是否渲染特定的内容  
##### 使用方法  
``` java 
 <c:if test="${something==somethingElse}">  
    要执行的内容  
 </c:if>  
 ```  
&nbsp;&nbsp;&nbsp;&nbsp;当test指定的条件为真时，该标签内嵌的内容才会被执行;该标签没有与之对应复杂的if-else逻辑 

##### 2.2.3 ` <c:choose> `、 ` <c:when> ` 、 ` <c:otherwise> `  标签
&nbsp;&nbsp;&nbsp;&nbsp;由于<c:if>标签没有与之对应的<c:else>标签，所以较为复杂的if-else逻辑就必须由这几个标签来完成。  
##### 使用方法  
``` java 
 <c:choose>  
    <c:when test="${something}">  if执行部分 </c:when>  
    <c:when test="${somethingElse}">  else if执行部分 </c:when>  
    ...
    <c:otherwise> else 执行部分 </c:otherwise>
 </c:choose>  
 ```  
 &nbsp;&nbsp;&nbsp;&nbsp; 需要注意的是，`  <c:choose>  `标签中最多只能有一个`<c:otherwise> `标签，而且只能位于最后

##### 2.2.4 ` <c:forEach> `  标签
&nbsp;&nbsp;&nbsp;&nbsp;该标签常用于遍历某些集合或数组，非常有用。  
##### 使用方法  
``` java 
 <c:forEach items="${users }" var="user">  
    <span>用户名：${user.username }</span>
    <span>年  龄：${user.age }</span>
    ...
 </c:forEach>  
 ```  
 &nbsp;&nbsp;&nbsp;&nbsp; 需要注意的是，这里有两个属性在执行遍历操作时非常的重要，一个是items属性，一个是var属性。其中items属性指定需要遍历的对象，相当于Java中for-each循环中第二个参数；var指定迭代变量，相当于for-each循环中第一个参数。
  &nbsp;&nbsp;&nbsp;&nbsp;  一般在显示列表信息时，可以与` <c:choose> `标签配套使用，如下： 
  ``` java 
 <c:choose>  
    <c:when test="${fn:length(users)==0}">
       <p>数据为空</p>
    </c:when>  
    <c:otherwise> 
       <c:forEach items="${users }" var="user">  
           <span>用户名：${user.username }</span>
           <span>年  龄：${user.age }</span>
           ...
       </c:forEach> 
    </c:otherwise>
 </c:choose>  
 ```  
