###1、版本问题，错误如下：
```java
org.springframework.beans.factory.BeanDefinitionStoreException: Failed to read 
candidatecomponent class: URL [jar:file:/D:/workspace/JavaSE/tms/target/tms/WEB-
INF/lib/mysql-connector-java-5.1.38.jar!/com/mysql/jdbc/JDBC42CallableStatement.class]; 
nested exception is org.springframework.core.NestedIOException: ASM ClassReader failed 
to parse class file - probably due to a new Java class file version that isn't supported 
yet: URL [jar:file:/D:/workspace/JavaSE/tms/target/tms/WEB-INF/lib/mysql-connector-java-
5.1.38.jar!/com/mysql/jdbc/JDBC42CallableStatement.class]; nested exception is 
java.lang.IllegalArgumentException
```
**这个是mysql驱动版本太高了**

```java
org.springframework.beans.factory.BeanDefinitionStoreException: Failed to read candidate 
component class: file [D:\workspace\JavaSE\tms\target\tms\WEB-
INF\classes\com\admin\action\LoginAction.class]; nested exception is 
org.springframework.core.NestedIOException: ASM ClassReader failed to parse class file - 
probably due to a new Java class file version that isn't supported yet: file 
[D:\workspace\JavaSE\tms\target\tms\WEB-INF\classes\com\admin\action\LoginAction.class]; 
nested exception is java.lang.IllegalArgumentException
```
**这个是`Idea jdk`环境太高`spring 3.x`在`jdk1.8` 上运行不了，配置如图**

编译级别：

![](https://dn-serical.qbox.me/15.png)

源代码级别：

![](https://dn-serical.qbox.me/16.png)

**两个都必须是1.7才能运行！！！**

###2、两个log冲突的问题：
```java
Annotation-specified bean name 'log' for bean class [com.mysql.jdbc.log.Log] conflicts 
with existing, non-compatible bean definition of same name and class 
[com.alibaba.druid.support.logging.Log]
```

错误的配置：

![](https://dn-serical.qbox.me/17.png)


正确的配置：

![](https://dn-serical.qbox.me/18.png)
