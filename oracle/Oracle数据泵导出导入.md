###准备工作
```sql
-- 创建DIRECTORY
CREATE DIRECTORY DMP_DIR AS 'D:\app\expdp_dir'
-- 授权
GRANT READ, WRITE ON DIRECTORY DMP_DIR TO SERICAL;
-- 查询DIRECTORY
SELECT * FROM ALL_DIRECTORIES A WHERE DIRECTORY_NAME = 'DMP_DIR';
-- 删除DIRECTORY
DROP DIRECTORY DMP_DIR ;
-- 用户DIRECTORY权限
SELECT * FROM USER_TAB_PRIVS;
```

###expdp导出
```bash
expdp serical/123456 directory=dmp_dir dumpfile=test.dmp logfile=test.log

连接到: Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
启动 "SERICAL"."SYS_EXPORT_SCHEMA_01":  serical/******** directory=dmp_dir dumpfile=test.dmp 
logfile=test.log
正在使用 BLOCKS 方法进行估计...
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE_DATA
使用 BLOCKS 方法的总估计: 0 KB
处理对象类型 SCHEMA_EXPORT/USER
处理对象类型 SCHEMA_EXPORT/SYSTEM_GRANT
处理对象类型 SCHEMA_EXPORT/ROLE_GRANT
处理对象类型 SCHEMA_EXPORT/DEFAULT_ROLE
处理对象类型 SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE
处理对象类型 SCHEMA_EXPORT/TABLE/INDEX/INDEX
处理对象类型 SCHEMA_EXPORT/TABLE/CONSTRAINT/CONSTRAINT
处理对象类型 SCHEMA_EXPORT/TABLE/INDEX/STATISTICS/INDEX_STATISTICS
处理对象类型 SCHEMA_EXPORT/TABLE/COMMENT
. . 导出了 "SERICAL"."TEST"                              5.445 KB       2 行
已成功加载/卸载了主表 "SERICAL"."SYS_EXPORT_SCHEMA_01"
******************************************************************************
SERICAL.SYS_EXPORT_SCHEMA_01 的转储文件集为:
  D:\APP\EXPDP_DIR\TEST.DMP
作业 "SERICAL"."SYS_EXPORT_SCHEMA_01" 已于 14:56:18 成功完成
```
**NLOGFILE=Y表示不生成日志文件**

###impdp导入
```bash
impdp serical/123456 directory=dmp_dir dumpfile=test.dmp nologfile=y full=y

连接到: Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
已成功加载/卸载了主表 "SERICAL"."SYS_IMPORT_FULL_01"
启动 "SERICAL"."SYS_IMPORT_FULL_01":  serical/******** directory=dmp_dir 
dumpfile=test.dmp nologfile=y full
处理对象类型 SCHEMA_EXPORT/USER
ORA-31684: 对象类型 USER:"SERICAL" 已存在
处理对象类型 SCHEMA_EXPORT/SYSTEM_GRANT
处理对象类型 SCHEMA_EXPORT/ROLE_GRANT
处理对象类型 SCHEMA_EXPORT/DEFAULT_ROLE
处理对象类型 SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE_DATA
. . 导入了 "SERICAL"."TEST"                            5.445 KB       2 行
作业 "SERICAL"."SYS_IMPORT_FULL_01" 已经完成, 但是有 1 个错误 (于 16:08:55 完成)
```

###user(serical)不存在时直接导入
```bash
impdp system/123456 directory=dmp_dir dumpfile=test.dmp nologfile=y 
remap_schema=serical:serical full=y

连接到: Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
已成功加载/卸载了主表 "SYSTEM"."SYS_IMPORT_FULL_01"
启动 "SYSTEM"."SYS_IMPORT_FULL_01":  system/******** directory=dmp_dir dumpfile=test.dmp 
nologfile=y remap_schema=serical:serical full=y
处理对象类型 SCHEMA_EXPORT/USER
处理对象类型 SCHEMA_EXPORT/SYSTEM_GRANT
处理对象类型 SCHEMA_EXPORT/ROLE_GRANT
处理对象类型 SCHEMA_EXPORT/DEFAULT_ROLE
处理对象类型 SCHEMA_EXPORT/PRE_SCHEMA/PROCACT_SCHEMA
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE
处理对象类型 SCHEMA_EXPORT/TABLE/TABLE_DATA
. . 导入了 "SERICAL"."TEST"                            5.445 KB       2 行
作业 "SYSTEM"."SYS_IMPORT_FULL_01" 已于 16:15:16 成功完成
```
**导入完成后即可使用serical用户登录，查看数据**
**Oracle官方文档说明**
[expdp](http://docs.oracle.com/cd/B19306_01/server.102/b14215/dp_export.htm)
[impdp](http://docs.oracle.com/cd/B19306_01/server.102/b14215/dp_import.htm)