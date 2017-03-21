### 一、查询锁
```sql
SELECT L.SESSION_ID || ',' || S.SERIAL#
  FROM V$LOCKED_OBJECT L, V$SESSION S
 WHERE S.SID = L.SESSION_ID
```
### 二、解锁
```sql
ALTER SYSTEM KILL SESSION '28,3706';
```

### 三、修改连接数
```sql
-- session数
SQL> show parameter session
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
java_max_sessionspace_size           integer     0
java_soft_sessionspace_limit         integer     0
license_max_sessions                 integer     0
license_sessions_warning             integer     0
session_cached_cursors               integer     50
session_max_open_files               integer     10
sessions                             integer     248
shared_server_sessions               integer     

-- process数
SQL> show parameter process
NAME                                 TYPE        VALUE
------------------------------------ ----------- ------------------------------
aq_tm_processes                      integer     0
cell_offload_processing              boolean     TRUE
db_writer_processes                  integer     1
gcs_server_processes                 integer     0
global_txn_processes                 integer     1
job_queue_processes                  integer     0
log_archive_max_processes            integer     4
processes                            integer     150


-- 已使用session数
SQL> select count(*) from v$session;
  COUNT(*)
----------
       141

-- 已使用process数
SQL> select count(*) from v$process;
  COUNT(*)
----------
       143

-- 修改最大进程数
SQL> alter system set processes=500 scope=spfile;
System altered
```