### 问题
```sql
SQL> alter database open;
alter database open
*
第 1 行出现错误:
ORA-01157: 无法标识/锁定数据文件 36 - 请参阅 DBWR 跟踪文件
ORA-01110: 数据文件 36:
'E:\ORACLE\APP\ADMINISTRATOR\ORADATA\ORCL\ZHMYRP_2017.DBF'
```

### 解决方法
```sql
SQL> alter database datafile 'E:\ORACLE\APP\ADMINISTRATOR\ORADATA\ORCL\ZHMYRP_2017.DBF' offline drop;
数据库已更改。
```

### 验证
```sql
# 实例状态
SQL> select status from v$instance;
STATUS
------------
MOUNTED

# open database
SQL> alter database open;
数据库已更改。

# open后的状态
SQL> select status from v$instance;
STATUS
------------
OPEN
```