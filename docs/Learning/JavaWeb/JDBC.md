### 连接数据库

> 连接数据库之前需要先导入依赖包

在web.xml文件中导入依赖包

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.23</version>
</dependency>
```

在新建的类中连接数据库

```java
import java.sql.*;
public class TestDB {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //连接设置
        String url="jdbc:mysql://localhost:3306/urlclass?useUnicode=true&charsetEncoding=utf-8;";//urlclass 这里是要连接的数据库名
        String username="root";
        String pw="******";
        Class.forName("com.mysql.cj.jdbc.Driver");//加载驱动
        Connection connection = DriverManager.getConnection(url, username, pw);
        //查询操作
        Statement statement = connection.createStatement();
        String sql="select * from test;";
        ResultSet set = statement.executeQuery(sql);//查询并获得结果集 增删改用executeUpdate函数 会返回受影响的行数
        while (set.next()){
            System.out.println(set.getString("test_name"));
        }
        //关闭连接
        set.close();
        statement.close();
        connection.close();
    }
}
```

### 预编译

```java
import java.sql.*;
public class TestDB {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //连接设置
        String url="jdbc:mysql://localhost:3306/urlclass?useUnicode=true&charsetEncoding=utf-8;";//urlclass 这里是要连接的数据库名
        String username="root";
        String pw="******";
        Class.forName("com.mysql.cj.jdbc.Driver");//加载驱动
        Connection connection = DriverManager.getConnection(url, username, pw);
        //预编译方式插入元组
        String sql2="insert into test(test_name,test_value1,test_value2)value(?,?,?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql2);
        preparedStatement.setString(1,"zhang");
        preparedStatement.setString(2,"kkk");
        preparedStatement.setString(3,"lll");
        int i = preparedStatement.executeUpdate();
        if(i>0){
            System.out.println("插入成功");
        }
        //关闭连接
        preparedStatement.close();
        connection.close();
    }
}
```

### JDBC事务

使用SQL语句直接书写事务的方式：

```mysql
start transaction;

update test set test_value1="123" where test_name="king";
update test set test_value2="456" where test_name="king";

commit;
```

在java中需要先关闭自动提交

```java
Connection connection = DriverManager.getConnection(url, username, pw);
try {
    connection.setAutoCommit(false);
    connection.prepareStatement("update test set test_value1='123' where test_name='king';").executeUpdate();
    int i=1/0;
    connection.prepareStatement("update test set test_value2='456' where test_name='king';").executeUpdate();
    connection.commit();
}catch (Exception e){
    connection.rollback();
}finally {
    connection.close();
}
```





