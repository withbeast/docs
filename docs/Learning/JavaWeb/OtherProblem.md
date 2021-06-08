### Junit

> 导入Junit依赖包之后就可以直接对单个函数进行测试

web.xml中导入Junit依赖包

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

只需要在需要测试的方法上面添加注解即可

```java
@Test
public void test(){
    System.out.println("nihao");
}
```

*注意@Test注解只有在方法上有效*