### JSP

> jsp最终也会被转化为一个servlet（java类）
>
> jsp转化为java代码时，代码块中的java语句直接保留，html标签则会使用out.write方法输出。
>
> 所以jsp的脚本片段都会直接生成在函数_jspService中，所以注意jsp脚本片段的作用域。

##### 导入依赖

```xml
<!--Servlet依赖-->
<!-- https://mvnrepository.com/artifact/jakarta.servlet/jakarta.servlet-api -->
<dependency>
    <groupId>jakarta.servlet</groupId>
    <artifactId>jakarta.servlet-api</artifactId>
    <version>5.0.0</version>
    <scope>provided</scope>
</dependency>
<!--JSP依赖-->
<!-- https://mvnrepository.com/artifact/jakarta.servlet.jsp/jakarta.servlet.jsp-api -->
<dependency>
    <groupId>jakarta.servlet.jsp</groupId>
    <artifactId>jakarta.servlet.jsp-api</artifactId>
    <version>3.0.0</version>
    <scope>provided</scope>
</dependency>
<!--JSTL标准标签库-->
<!-- https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api -->
<dependency>
    <groupId>jakarta.servlet.jsp.jstl</groupId>
    <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
    <version>2.0.0</version>
</dependency>
<!--standard标签库-->
<!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
<dependency>
    <groupId>org.apache.taglibs</groupId>
    <artifactId>taglibs-standard-impl</artifactId>
    <version>1.2.5</version>
</dependency>

```

##### JSP基础语法

```jsp
<!-- 注释  -->
<%-- jsp注释 （jsp的注释不会出现在html文档中） --%>

<!-- jsp脚本片段  -->
<%	//片段内注释
    String name="kings";
    setName(name);
%>
<%=getName()%>

<!-- 循环html标签  -->
<%
    for(int i=0;i<3;i++){
%>
<h1>循环三次h1标签 <%=i%></h1>
<%
    }
%>

<!-- JSP声明 -->
<%! //JSP声明会被编译到java的类中
    public String Sname;
    public void setName(String n){
        Sname=n;
    }
    public String getName(){
        return Sname;
    }
%>

<!-- EL表达式  -->
<% pageContext.setAttribute("name", name); %>
<h1>name: ${name}</h1>

<!-- 转发  -->
<%
	pageContext.forward("/index.jsp");
%>

```

##### jsp指令

```jsp
<!-- 乱码问题  -->
<%@page contentType="text/html; charset=UTF-8" language="java"%>
<!-- 导包  -->
<%@page import="java.util.Date"%>
<%= new Date()%>

<!-- 错误页面  -->
<%@page errorPage="error/500.jsp" %>
<%@page isErrorPage="true" %><!-- 声明这是一个错误页面  -->

<!-- 导入页面  -->
<%@include file="common/header.jsp" %><!-- include方式 生成java文件时会将页面合并 函数的作用域相同 -->
<jsp:include page="common/header.jsp"/><!-- jsp标签方式 生成java文件时会以静态资源的方式将页面加载如类中，然后再拼接页面 函数作用域不同-->

```

*错误界面配置*

错误界面同样可以在web.xml中配置

```xml
<error-page>
	<error-code>404</error-code>
    <location>/error</location>
</error-page>
```

##### 九大内置对象

* request（HttpServletRequest）
* response（HttpServletResponse）
* pageContext（PageContext）
* application（ServletContext）
* session（HttpSession)
* config（ServletConfig）
* out（JspWriter）
* page（指向该java类，其父类为HttpJspBase）
* exception（Throwable）

其中_jspService方法中的九大内置对象的形式

```java
public void _jspService(final HttpServletRequest request,final HttpServletResponse response){
	final PageContext pageContext=_jspxFactory.getPageContext(this, request, response,null, false, 8192, true);
	final ServletContext application = pageContext.getServletContext();
	final ServletConfig config = pageContext.getServletConfig();
	JspWriter out = pageContext.getOut();
	final java.lang.Object page = this;
```

>pageContext、request、session、application都可以用来存取数据
>
>pageContext 保存的的数据只在一个页面中有效
>
>request 保存的数据只在一次请求中有效 请求转发会携带这个数据
>
>session 保存的数据只在一次会话中有效 （直到关闭浏览器）
>
>application 保存的数据在服务器中 （直到服务器关机）

##### JSP标签库

```jsp
<jsp:include page="common/header.jsp"/><!--整合页面-->
<!-- 转发 -->
<jsp:forward page="error.jsp">
	<jsp:param name="" value=""></jsp:param>
    <jsp:param name="" value=""></jsp:param>
</jsp:forward>
```

*JSTL(JSP标准标签库)*

> 核心标签是最常用的 JSTL标签

```jsp
<!-- 在jsp中引入核心标签库 core -->
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!-- 在jsp中引入格式化标签库 format -->
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!-- 在jsp中引入SQL标签库 sql -->
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
<!-- 在jsp中引入XML标签库 xml -->
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
```

##### EL表达式



