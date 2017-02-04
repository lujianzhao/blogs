###一：component-scan
```java
<context:component-scan base-package="com.serical.action"/>
```
`base-package`，如果最后的包下面无子包了，不能写.*这种形式，否则扫描不到`Controller`，建议直接写到父包就可以了，不想注册哪个组件可以通过下面的方式。
```java
<context:exclude-filter type="" expression=""/>
<context:include-filter type="" expression=""/>
```
如果使用了`exclude-filter`和`include-filter`则要如下配置，否则上面的filter无效
```java
 use-default-filters="false"
```

###二：HTTP CODE 406
增加转换器`MappingJacksonHttpMessageConverter`
```xml
<mvc:annotation-driven>
        <mvc:message-converters>
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="supportedMediaTypes">
                    <list>
                        <value>application/json;charset=UTF-8</value>
                    </list>
                </property>
            </bean>
            <bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"/>
        </mvc:message-converters>
    </mvc:annotation-driven>
```
需要增加jar包
http://central.maven.org/maven2/org/codehaus/jackson/jackson-mapper-asl/1.4.2/jackson-mapper-asl-1.4.2.jar 
http://central.maven.org/maven2/org/codehaus/jackson/jackson-core-asl/1.4.2/jackson-core-asl-1.4.2.jar
Maven地址
```xml
<!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-mapper-asl -->
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-mapper-asl</artifactId>
    <version>1.4.2</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.codehaus.jackson/jackson-core-asl -->
<dependency>
    <groupId>org.codehaus.jackson</groupId>
    <artifactId>jackson-core-asl</artifactId>
    <version>1.4.2</version>
</dependency>
```