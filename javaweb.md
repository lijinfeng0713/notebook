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
