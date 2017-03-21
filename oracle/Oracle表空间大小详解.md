### 一、问题
sql需要索引创建索引时提示xxx表空不足，只好临时建到另一个表空间上，先解决了问题。生产库一个表空间已经到了30G的大小，后来想起随着时间表空间总会满的，到时候系统出了问题就不好解决了，先了解下如何解决该问题。

### 二、了解相关知识
按照Oracle文档的描述，每个`datafile`的最大容量为`(2^22-1)`个`block`，即`4194303`个`block`，而当前数据库的`block大`小是`8k`，
```sql
SQL> SHOW PARAMETER DB_BLOCK_SIZE;
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
db_block_size                        integer     8192
```
也就是说最大的文件大小是`32G`，要建`100G`的`datafile`就不行了。 
也就是说，以Oracle的限制，如果要建普通的`datafil`e，最大的大小就 
`(2^22-1)*32K = 128G` (注：Oracle最大支持block为32k)。存在这个限制是因为`Oracle`的内部`ROWID`使用22位2进制数来存储不同的`block`号，
所以22位最多代表`(2^22-1)`个`block`。

### 三、测试
```sql
--这里创建一个表空间测试下
CREATE TABLESPACE SERICAL
DATAFILE 'D:\app\serical\oradata\god\SERICAL.DBF'
SIZE 100M
AUTOEXTEND ON;
-- 删除表空间
DROP TABLESPACE SERICAL1 INCLUDING CONTENTS CASCADE CONSTRAINTS
-- 创建serical用户
CREATE USER SERICAL IDENTIFIED BY 123456
DEFAULT TABLESPACE SERICAL;
-- 授权
GRANT DBA, RESOURCE, CONNECT TO SERICAL;
-- 创建测试表TEST
CREATE TABLE TEST (ID INTEGER, NAME VARCHAR2(100));
-- 插入一条数据
INSERT INTO TEST VALUES (1, 'SERICAL');
-- 连续疯狂插入
INSERT INTO TEST SELECT * FROM TEST;
-- 记录数
SQL> SELECT COUNT(*) FROM TEST;

  COUNT(*)
----------
     65536

-- 查询表空间使用情况，可以看出，使用了3M空间，384个block，384*8k/1024=3M
SQL> SELECT T.TABLESPACE_NAME, SUM(T.BYTES)/1024/1024||'M' AS USED, SUM(T.BLOCKS)
  2  FROM DBA_SEGMENTS T
  3  WHERE T.TABLESPACE_NAME = 'SERICAL'
  4  GROUP BY T.TABLESPACE_NAME;

TABLESPACE_NAME                USED    SUM(T.BLOCKS)
------------------- --------------     -------------
   SERICAL                     3M          384

-- 这时候SERICAL表空间最大值
SQL> SELECT T.TABLESPACE_NAME,
  2  SUM(T.BYTES)/1024/1024 AS BYTES,
  3  SUM(T.BLOCKS) AS BLOCKS,
  4  SUM(T.MAXBYTES)/1024/1024 AS MAXBYTES,
  5  SUM(T.MAXBLOCKS) AS MAXBLOCKS
  6  FROM DBA_DATA_FILES T
  7  WHERE T.TABLESPACE_NAME = 'SERICAL'
  8  GROUP BY T.TABLESPACE_NAME;
TABLESPACE_NAME    BYTES     BLOCKS     MAXBYTES    MAXBLOCKS
---------------- ---------- ---------- ----------   ----------
SERICAL            1024     131072     32767.9843    4194302
```
随着数据插入表空间会自动扩容，未指定`MAXSIZE`或者`MAXSIZE UNLIMITED`，`Windows`下`NTFS`最大`32G`

```sql
-- 未达到32G前可以使用下面sql来弥补创建表空间时的不足
ALTER DATABASE DATAFILE 'D:\app\serical\oradata\god\SERICAL.DBF' RESIZE 200M;
ALTER DATABASE DATAFILE 'D:\app\serical\oradata\god\SERICAL.DBF' AUTOEXTEND ON MAXSIZE UNLIMITED;
```

**接近32G时，比如生产库的30G，在不动原来表空间的情况下可以采用添加DATAFILE的方式扩充表空间**
```sql
SQL> ALTER TABLESPACE SERICAL ADD DATAFILE 'D:\app\serical\oradata\god\SERICAL1.DBF' 
    SIZE 100M AUTOEXTEND ON;
Tablespace altered
-- 这时的表空间最大值，可以看出扩大了一倍
SQL> SELECT T.TABLESPACE_NAME,
  2  SUM(T.BYTES)/1024/1024 AS BYTES,
  3  SUM(T.BLOCKS) AS BLOCKS,
  4  SUM(T.MAXBYTES)/1024/1024 AS MAXBYTES,
  5  SUM(T.MAXBLOCKS) AS MAXBLOCKS
  6  FROM DBA_DATA_FILES T
  7  WHERE T.TABLESPACE_NAME = 'SERICAL'
  8  GROUP BY T.TABLESPACE_NAME;
TABLESPACE_NAME  BYTES     BLOCKS      MAXBYTES   MAXBLOCKS
--------------- ---------- ---------- ---------- ----------
SERICAL          300      38400        65535.9687    8388604
```

### 应用
```sql
CREATE TABLESPACE PERSONAL_DATA
LOGGING
DATAFILE 'D:\app\serical\oradata\orcl\PERSONAL_DATA.DBF'
SIZE 1G
AUTOEXTEND ON;

ALTER TABLESPACE PERSONAL_DATA
ADD DATAFILE 'D:\app\serical\oradata\orcl\PERSONAL_DATA1.DBF'
SIZE 100M
AUTOEXTEND ON;

CREATE TABLESPACE SJXF
LOGGING
DATAFILE 'D:\app\serical\oradata\orcl\SJXF.DBF'
SIZE 1G
AUTOEXTEND ON;


ALTER TABLESPACE SJXF
ADD DATAFILE 'D:\app\serical\oradata\orcl\SJXF1.DBF'
SIZE 100M
AUTOEXTEND ON;

CREATE USER LOUDI IDENTIFIED BY "123456a"
DEFAULT TABLESPACE PERSONAL_DATA;

GRANT DBA, RESOURCE, CONNECT TO LOUDI;

CREATE DIRECTORY expdp_dir AS 'D:\app\expdp_dir';

GRANT READ, WRITE ON DIRECTORY expdp_dir TO LOUDI;
```