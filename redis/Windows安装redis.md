### 一、单实例安装
1. 下载[Redis-x64-3.2.100.msi](https://github.com/MicrosoftArchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.msi)
一路下一步就OK了，常用配置如下：
```ini
bind 127.0.0.1
port 6379
appendonly no
requirepass foobared
```
2. 有些版本服务器安装不上，则需要下载[压缩包](https://github.com/MicrosoftArchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.zip)手动安装了,
命令行（**管理员**）进入解压的目录
```ini
# 如果安装失败尝试redis.windows-service.conf的绝对路径
redis-server --service-install redis.windows-service.conf
```
### 二、多实例安装
修改`redis.windows-service.conf`中的`port`端口，必须与其他实例端口不一样。
```ini
port 6379
```
参照上面第二步，命令行（**管理员**）进入解压的目录，增加`--service-name`参数去重命名`服务名`
```ini
redis-server.exe --service-install redis.windows-service.conf --service-name redis1
```