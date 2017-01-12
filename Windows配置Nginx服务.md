###问题背景
公司服务器为windows系统，几次停电关机后门户网站由于Nginx并没有重启而挂了。

###解决方法
nginx下载地址：http://nginx.org/download/nginx-1.10.1.zip<br>
github地址：https://github.com/kohsuke/winsw<br>
下载地址：http://repo.jenkins-ci.org/releases/com/sun/winsw/winsw/ 我使用的是1.18<br>

###1、配置
下载`winsw-1.18-bin.exe`放到`Nginx`根目录下重命名为`app.exe`，新建一个`app.xml`，Google了网上配置如下：
```xml
<service>
    <id>nginx</id>
    <name>nginx</name>
    <description>nginx</description>
    <executable>c:\nginx\nginx.exe</executable>
    <logpath>c:\nginx\</logpath>
    <logmode>roll</logmode>
    <depend></depend>
    <startargument>-p c:\nginx</startargument>
    <stopargument>-p c:\nginx -s stop</stopargument>
</service>
```
###2、安装服务，进入Nginx根目录使用`app.exe install`安装服务，其他命令请看`github`文档

###3、在使用过程中，下面两个-p后面不能有空格，否则报错找不到2个日志文件
```xml
<startargument>-p c:\nginx</startargument>
<stopargument>-p c:\nginx -s stop</stopargument>
```

###4、在服务里面停止不了Nginx2个进程，仔细看了[github](https://github.com/kohsuke/winsw#stoptimeout)文档
> stoptimeout
When the service is requested to stop, winsw first attempts to send Ctrl+C signal to the process, then wait for up to 15 seconds for the process to exit by itself gracefully. A process failing to do that (or if the process does not have a console), then winsw resorts to calling TerminateProcess API to kill the service instantly.

最终去掉了`stopargument`参数

###5、使用workingdirectory来指定当前PATH
> Some services need to run with a working directory specified. To do this, specify a element like this:
```xml
<workingdirectory>C:\application</workingdirectory>
```

最终配置如下：
```xml
<service>
    <id>nginx</id>
    <name>nginx</name>
    <description>nginx</description>
    <workingdirectory>D:\protal\nginx</workingdirectory>
    <executable>nginx.exe</executable>
    <logmode>roll</logmode>
</service>
```
可满足创建Windows服务，开机启动，停止Nginx进程