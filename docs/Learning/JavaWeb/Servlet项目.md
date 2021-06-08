### 配置问题

##### 父子工程

创建一个简单的maven项目删去src目录，然后在项目中创建一个module，在module的pom.xml文件中确认其中是否有parent标签，在主项目中的pom.xml文件中module标签。

parent标签:

```xml
<parent>
   <groupId>org.example</groupId>
   <artifactId>maven01</artifactId>
   <version>1.0-SNAPSHOT</version>
</parent>
<!-- 如果有parent标签那么需要把project标签内的groupId标签删除 留下artifactId标签 -->
<!-- 如果没有parent标签，module中的项目将无法引用父工程的依赖 -->
```

module标签：

```xml
<modules>
    <module>servlet01</module>
</modules>
```

##### Mapping问题

web.xml文件中的mapping配置

```xml
<servlet>
    <servlet-name>kings01</servlet-name>
    <servlet-class>com.withbeast.servlet.myServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>kings01</servlet-name>
    <url-pattern>/kings01</url-pattern>
</servlet-mapping>
```

url-pattern中如果使用通配符 * ，可以让让目录的任意路径的指向该servlet。mapping标签可以配置多个，指向同一个servlet

### ServletContext

##### 共享信息

两个servlet通过ServletContext共享信息

ServletA：

```java
//通过ServletContext设置信息
ServletContext context = this.getServletContext();
String name="my";
context.setAttribute("name",name);
```

ServletB：

```java
//通过ServletContext取出信息
ServletContext context = this.getServletContext();
String name =(String) context.getAttribute("name");
```

##### 读取文件

在resource目录下面创建文件db.properties

```java
InputStream is = context.getResourceAsStream("/WEB-INF/classes/db.properties");//获取文件流
Properties prop=new Properties();
prop.load(is);//加载
String un = (String)prop.get("username");//获得db.properties文件中的信息
writer.println(un);
```

### Request和Response的应用

##### Response对象

```java
//重要的函数
HttpServletResponse resp;
resp.getOutputStream();//获取响应对象的输出流，可以利用流向客户端传输数据
resp.setContentType("image/jpeg");
resp.setDateHeader("expires",-1);
resp.setHeader("Cache-Control","no-cache");
resp.sendRedirect("/servlet01/verify");//实现重定向
```

##### Request对象

```java
//重要的函数
String username = req.getParameter("username");//获取表单中的参数
```

##### 下载文件

```java
resp.setHeader("Content-Disposition","attachment;filename=logo.png");//设置响应头，
int len=0;
byte[] buffer=new byte[1024];
ServletContext context = this.getServletContext();
InputStream is = context.getResourceAsStream("/WEB-INF/classes/logo.png");//获取资源的输入流
ServletOutputStream out=resp.getOutputStream();//获取响应对象的输出流
while((len=is.read(buffer))>0){//流操作
   out.write(buffer);
}
is.close();
out.close();
/*
文件名如果是中文的话可能会产生乱码
String c=URLEncoder.encode("你好","UTF-8");
*/
```

##### 验证码

```java
	private String RandNum(){
        Random rand = new Random();
        int i = rand.nextInt(99999);
        return String.format("%05d", i);
    }
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setHeader("refresh","3");
        BufferedImage image=new BufferedImage(80,20,BufferedImage.TYPE_INT_RGB);
        Graphics g = image.getGraphics();
        g.setColor(Color.white);
        g.fillRect(0,0,60,20);
        g.setColor(Color.BLACK);
        g.setFont(new Font(null,Font.ITALIC,20));
        g.drawString(RandNum(),0,20);

        resp.setContentType("image/jpeg");
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","no-cache");
        ImageIO.write(image,"jpg",resp.getOutputStream());

    }
```

##### 表单提交

index.jsp中的代码

```java
<form action="${pageContext.request.contextPath}/login" method="get">
    username:<input type="text" name="username"><br>
    <input type="submit">
</form>
```

servlet中的响应代码

```java
req.setCharacterEncoding("utf-8");
String username = req.getParameter("username");
resp.getWriter().println(username);
```

