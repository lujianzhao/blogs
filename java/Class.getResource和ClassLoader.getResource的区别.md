```java
package com.power.bank.payment.action;

public class Test {

    public static void main(String[] args) {
        // 查找当前类所在包下的springMVC.xml
        System.out.println(Test.class.getResource("springMVC.xml"));
        // 获取当前class文件
        System.out.println(Test.class.getResource("Test.class"));
        // "/"开头的，查询classpath根目录下的springMVC.xml
        System.out.println(Test.class.getResource("/springMVC.xml"));
        // 获取别的类路径需要带"/"
        System.out.println(Test.class
            .getResource("/com/theta/common/bean/EasyUiTreeNode.class"));

        // "/"开头都是null，查找classpath根目录下的springMVC.xml
        System.out.println(Test.class.getClassLoader().getResource("springMVC.xml"));
        // 获取当前class文件需要带包名
        System.out.println(Test.class.getClassLoader()
            .getResource("com/theta/common/bean/EasyUiTreeNode.class"));
    }

}
```
从上面可以看出：
`class.getResource("/") == class.getClassLoader().getResource("") `
其实，`Class.getResource`和`ClassLoader.getResource`本质上是一样的，都是使用`ClassLoader.getResource`加载资源的。下面请看一下`jdk`的`Class`源码:
```java
public java.net.URL getResource(String name)
{
    name = resolveName(name);
    ClassLoader cl = getClassLoader0();
    if (cl==null)
    {
        // A system class.
        return ClassLoader.getSystemResource(name);
    }
    return cl.getResource(name);
}
```
至于为什么`lass.getResource(String path)`中`path`可以/开头，是因为在`name = resolveName(name);`进行了处理：
```java
 private String resolveName(String name)
    {
        if (name == null)
        {
            return name;
        }
        if (!name.startsWith("/"))
        {
            Class c = this;
            while (c.isArray()) {
                c = c.getComponentType();
            }
            String baseName = c.getName();
            int index = baseName.lastIndexOf('.');
            if (index != -1)
            {
                name = baseName.substring(0, index).replace('.', '/')
                    +"/"+name;
            }
        } else
        {//如果是以"/"开头，则去掉
            name = name.substring(1);
        }
        return name;
    }
```