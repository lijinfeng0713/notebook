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
`` java 
<c:if test="${something==somethingElse}">  
    要执行的内容  
 </c:if> ``
