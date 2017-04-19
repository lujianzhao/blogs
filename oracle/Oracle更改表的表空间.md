### 一、当前表空间
```sql
SQL> select t.TABLESPACE_NAME from user_tables t where t .TABLE_NAME = 'AA';
TABLESPACE_NAME
------------------------------
USERS
```

### 二、更改表空间
```sql
SQL> ALTER TABLE AA MOVE TABLESPACE PERSONAL_DATA;
Table altered
```

### 三、查看表空间
```sql
SQL> select t.TABLESPACE_NAME from user_tables t where t .TABLE_NAME = 'AA';
TABLESPACE_NAME
------------------------------
PERSONAL_DATA
```