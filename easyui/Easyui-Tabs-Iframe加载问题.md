代码：

![](../static/21.png)

问题：会多次加载页面

![](../static/22.png)
 
解决方法：先设置iframe高度

![](../static/23.png)
 
等页面解析完毕再去加载iframe

![](../static/24.png)
 
结果：都只加载一次

![](../static/25.png)
 