### 一、下载相关文件
[MySQL压缩包](http://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.16-winx64.zip)

[压缩版安装文档](http://dev.mysql.com/doc/refman/5.7/en/windows-install-archive.html)

### 二、配置环境
1. 添加环境变量，将上面压缩包解压到`D:\Program Files\Mysql`，然后将`D:\Program Files\Mysql\bin`添加到环境变量PATH中； 
2. 复制解压目录下的`my-default.ini`文件，重命名为`my.ini`； 
3. 添加如下配置：

```bash
basedir = D:/Program Files/Mysql
datadir = D:/Program Files/Mysql/data
port = 3306
collation_server=utf8_general_ci
character_set_server=utf8
```

### 三、初始化数据库
命令如下：
```bash
#mysqld --initialize --user=mysql --console
D:\Program Files\Mysql\bin>mysqld.exe --initialize --user=mysql --console
2016-11-14T01:44:54.364486Z 0 [Warning] TIMESTAMP with implicit DEFAULT value is deprecated. Please use --explicit_defaults_for_timestamp server option (see documentation for more details).
2016-11-14T01:44:54.364486Z 0 [Warning] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'ERROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will be merged with strict mode in a future release.
2016-11-14T01:44:54.364486Z 0 [Warning] 'NO_AUTO_CREATE_USER' sql mode was not set.
2016-11-14T01:44:56.083163Z 0 [Warning] InnoDB: New log files created, LSN=45790
2016-11-14T01:44:56.591605Z 0 [Warning] InnoDB: Creating foreign key constraint system tables.
2016-11-14T01:44:56.770716Z 0 [Warning] No existing UUID has been found, so we assume that this is the first time that this server has been started. Generating a new UUID: f24a83ec-aa0b-11e6-85ea-d017c2a9c3f4.
2016-11-14T01:44:56.801987Z 0 [Warning] Gtid table is not ready to be used. Table 'mysql.gtid_executed' cannot be opened.
2016-11-14T01:44:56.813028Z 1 [Note] A temporary password is generated for root@localhost: _;9#8N_VzmxV
```
**在控制台消息尾部会出现随机生成的初始密码，记下来**

### 四、添加为系统服务
```bash
D:\Program Files\Mysql\bin>mysqld --install MySQL
Service successfully installed.

# 移除服务
D:\Program Files\Mysql\bin>mysqld.exe --remove
Service successfully removed.
```

### 五、登录并修改密码
```bash
#回车执行后，输入刚才记录的随机密码
mysql -u root -p 

#执行成功后，控制台显示 mysql>，则表示进入mysql，修改密码
mysql> set password for root@localhost = password('123456');
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

### 六、可能出现的错误
无法启动此程序，因为计算机中丢失MSVCR120.dll。请重新安装该程序已解决问题

### 七、解决方法
[下载](https://www.microsoft.com/en-us/download/details.aspx?id=40784)安装重新进行第三步