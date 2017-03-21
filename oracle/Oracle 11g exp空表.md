### 1、知识说明
Oracle 11g中推出的“延迟段创建”（`Deferred Segment Creation`）特性，以及当我们使用这种特性时，需要注意的问题。

`Deferred Segment Creation`
在Oracle中，“表空间（`Tablespace`）、段（`Segment`）、分区（`Extent`）和块（`Block`）”是逻辑存储结构的四个层次。
对数据表而言，通常是由一个或者多个段对象（分区表）`Segment`组成。也就是说，在数据表创建的时刻，Oracle会创建一个数据段`Segment`对象与之对应。
当`Segment`创建之后，Oracle空间管理机制会根据需要分配至少一个`extent`作为初始化。每个`extent`的大小需要根据不同`tablespace`进行配置。
但是在11g之前，数据表的创建同时，就发生了空间`Segment`分配的过程。但是在Oracle 11g中，引入了`Deferred Segment Creation`特性。
首先我们创建一个数据表`justForTest`，来观察数据库是否为此表分配`segment`。
```sql
SQL> create table justForTest(test1 varchar2(2));
Table created
```
但是，对应的段`segment`对象，却没有创建出来，如下：
```sql
select segment_name, BYTES, BLOCKS, EXTENTS  from user_segments where segment_name='JUSTFORTEST';
SEGMENT_NAME    BYTES       BLOCKS    EXTENTS
------------ ---------- ---------- ----------
```
这就是在Oracle 11g中引入的延迟段生成。一个数据表，如果刚刚创建出来的时候没有数据加入。Oracle是不会为这个对象创建相应的段结构，也就不会分配对应的空间。 
使用`dbms_metadata`抽取出数据表的DDL语句，可以发现端倪，如下：
```sql
CREATE TABLE "test"."JUSTFORTEST" 
   (    "TEST1" VARCHAR2(2)
   ) SEGMENT CREATION DEFERRED 
  PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 NOCOMPRESS LOGGING
  TABLESPACE "testNS";
```
使用DDL语句可以获取到创建数据表的所有语句参数，包括默认参数。其中，我们发现了一个在过去版本中没有参数“`SEGMENT CREATION DEFERRED`”，
该参数就表示在数据表创建中使用延迟段生成。

那么，在什么时点上Oracle才会创建对象呢？只要插入一条数据到数据表中，无论是否commit，都会伴随着Oracle对数据表段的创建操作。
```sql
SQL> insert into justForTest values('1');
1 row inserted
SQL> select segment_name, BYTES, BLOCKS, EXTENTS  from user_segments 
        where segment_name='JUSTFORTEST';
SEGMENT_NAME    BYTES       BLOCKS    EXTENTS
------------ ---------- ---------- ----------
JUSTFORTEST     65536          8        1
```
Oracle推出`Deferred Segment Creation`的出发点很单纯，就是出于对象空间节省的目的。如果一个空表从来就没有使用过，创建`segment`对象，
分配空间是“不合算”的，所以提出推迟段创建的时间点。

### 2、永久解决方案，但对之前已存在表无效
```sql
-- 为true表示使用延迟创建segment
SQL> SHOW PARAMETER DEFERRED_SEGMENT_CREATION
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
deferred_segment_creation            boolean     TRUE
-- 修改为false，以后创建表就不会使用延迟创建
SQL> ALTER SYSTEM SET DEFERRED_SEGMENT_CREATION = FALSE;
System altered
```

### 3、兼容以前创建的表
```sql
-- 查询需要手动分配segment的table
SELECT 'ALTER TABLE '||T.TABLE_NAME||' ALLOCATE EXTENT;' FROM USER_TABLES T 
        WHERE T.SEGMENT_CREATED = 'NO' OR T.NUM_ROWS = 0;
-- 执行查询出的语句，即完成手动分配segment
ALTER TABLE XXXX ALLOCATE EXTENT;
```