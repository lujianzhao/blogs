Oracle的`Flashback Drop`闪回删除功能给出我们一种误DROP删除表的便捷恢复方式，实现这种功能的原理是Oracle的“回收站”（`RecycleBin`）功能。
注意，如果被删除的表原先是存放在`SYSTEM`系统表空间上，则不支持此功能。

###1.Flashback Drop功能
恢复被错误`drop`掉的表。当一张表被删除后，依然可以查看被drop表的内容，是通过查看回收站中的内容实现的。

###2.实现原理
被删除的表将被存在一个叫`recyclebin`回收站的地方，当`drop`掉表后，实际上就是将改表改了个名字。

###3.与回收站有关的视图
```sql
DBA_RECYCLEBIN
USER_RECYCLEBIN
RECYCLEBIN
```

###4.显示当前用户曾经被drop掉的表简短信息
```sql
-- 闪回功能是否开启由RECYCLEBIN决定，默认ON
SQL> SHOW PARAMETER RECYCLEBIN
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
recyclebin                           string      on
SQL> show recyclebin
ORIGINAL NAME    RECYCLEBIN NAME                OBJECT TYPE  DROP TIME
---------------- ------------------------------ ------------ -------------------
AA               BIN$dR7wTMsMQmOrRuxyPMkUMQ==$0 TABLE        2015-07-31:12:22:03
AA               BIN$C2SUbE0zQdyQ8kKVUIXzGw==$0 TABLE        2015-07-31:11:57:15
CTEST            BIN$iTZrphk7Raec1IuAGxPHAA==$0 TABLE        2015-08-21:12:29:59
```

###5.清除回收站内容的条件
* 1）表空间不足 
* 2）用户的空间配额不足 
* 3）`purge`命令 
* 4）使用`flashback`命令恢复表后，与之对应的回收站中的那条记录内容被清除。

###6.Flashback Drop语法
```sql
SQL> FLASHBACK TABLE AA TO BEFORE DROP;
SQL> FLASHBACK TABLE "BIN$iTZrphk7Raec1IuAGxPHAA==$0" TO BEFORE DROP;
```
上面两种方法都可以实现找回被删除表的功能。
**第一种方法是恢复到最后一次被删除的状态；**
**第二种方法则可以对回收站中具体的一个对象进行闪回，用于一张表被多次删除后的恢复场景。**

###7.Flashback Drop闪回删除功能实践
```sql
(1).创建测试表AA
SQL> CREATE TABLE AA AS SELECT * FROM ALL_OBJECTS;
表已创建。
SQL> SELECT COUNT(*) FROM AA;
  COUNT(*)
----------
     72355
     
(2).模拟drop掉AA表
SQL> DROP TABLE AA;
表已删除。

(3).查看回收站，这里看到ft_1表已经在回收站中了
SQL> SHOW RECYCLEBIN;
ORIGINAL NAME    RECYCLEBIN NAME                OBJECT TYPE  DROP TIME
---------------- ------------------------------ ------------ -------------------
AA               BIN$lZpcw9AdRrafbW2nsd5r9Q==$0 TABLE        2015-08-21:21:56:20
AA               BIN$dR7wTMsMQmOrRuxyPMkUMQ==$0 TABLE        2015-07-31:12:22:03
AA               BIN$C2SUbE0zQdyQ8kKVUIXzGw==$0 TABLE        2015-07-31:11:57:15
CTEST            BIN$iTZrphk7Raec1IuAGxPHAA==$0 TABLE        2015-08-21:12:29:59

(4).演示一下查询功能
SQL> SELECT COUNT(*) FROM "BIN$lZpcw9AdRrafbW2nsd5r9Q==$0";
  COUNT(*)
----------
     72355
     
(5).闪回被drop掉的表
SQL> FLASHBACK TABLE AA TO BEFORE DROP;
闪回完成。
SQL> SELECT COUNT(*) FROM AA;

  COUNT(*)
----------
     72355
     
（6）.存在多个表时恢复到某个具体的表，也可以使用下面的命令进行恢复。
SQL> DROP TABLE AA;
表已删除。

SQL> SHOW RECYCLEBIN;
ORIGINAL NAME    RECYCLEBIN NAME                OBJECT TYPE  DROP TIME
---------------- ------------------------------ ------------ -------------------

AA               BIN$P8v6E+YiQgWL7bFsiKiE9A==$0 TABLE        2015-08-21:22:02:47

AA               BIN$dR7wTMsMQmOrRuxyPMkUMQ==$0 TABLE        2015-07-31:12:22:03

AA               BIN$C2SUbE0zQdyQ8kKVUIXzGw==$0 TABLE        2015-07-31:11:57:15

CTEST            BIN$iTZrphk7Raec1IuAGxPHAA==$0 TABLE        2015-08-21:12:29:59

SQL> FLASHBACK TABLE "BIN$P8v6E+YiQgWL7bFsiKiE9A==$0" TO BEFORE DROP;
闪回完成。
SQL> SELECT COUNT(*) FROM AA;
  COUNT(*)
----------
     72355
OK，到这里，被删除的表便被顺利的恢复回来。
```

###8.清除回收站内容的方法
如果您确定、一定以及肯定不想恢复这些表的时候，可以使用以下方法对回收站进行清理。 
```sql
1）清除当前用户的回收站
SQL> purge recyclebin;
SQL> purge user_recyclebin;

2）清除指定表空间tbs_sec_d的回收站
SQL> purge tablespace tbs_secooler_d;

3）清除指定表空间tbs_sec_d，同时指定用户sec的回收站
SQL> purge tablespace tbs_sec_d user sec;

4）清除回收站中所有的内容（sys用户）
SQL> purge dba_recyclebin
```

###9.不产生回收站数据的同时drop表方法
这种方法是彻底删除表的方法，使用前要考虑清楚。
```sql
SQL> drop table ft_1 purge;
```

###10.小结
在使用`Flashback Drop`闪回删除功能之前要充分了解此项功能的实现原理，以及使用此项功能的条件和它的限制条件。
闪回删除功能为我们提供了表被误`DROP`后的便捷恢复手段。