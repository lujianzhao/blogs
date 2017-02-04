###Oracle EXP&IMP参数详解
exp&imp是Oracle自带的导入导出命令，运用它，即使不需要那结UI工具也能轻易的完成数据导出导入工作，下面是它们的参数：

###EXP参数详解
使用的格式是：EXP KEYWORD=value 或 KEYWORD=(value1,value2,…,valueN) 
其中USERID是必须的且为第一个参数

**关键字 备注**
* USERID 用户名and口令
* FULL 导出整个文件 (N)
* BUFFER 数据缓冲区的大小
* OWNER 所有者用户名列表
* FILE 输出文件 (EXPDAT.DMP)
* TABLES 表名列表
* COMPRESS 导入一个范围 (Y)
* RECORDLENGTH IO 记录的长度
* GRANTS 导出权限 (Y)
* INCTYPE 增量导出类型
* INDEXES 导出索引 (Y)
* RECORD 跟踪增量导出 (Y)
* ROWS 导出数据行 (Y)
* PARFILE 参数文件名
* CONSTRAINTS 导出限制 (Y)
* CONSISTENT 交叉表一致性
* LOG 屏幕输出的日志文件
* STATISTICS 分析对象 (ESTIMATE)
* DIRECT 直接路径 (N)
* TRIGGERS 导出触发器 (Y)
* FEEDBACK 显示每 x 行 (0) 的进度
* FILESIZE 各转储文件的最大尺寸
* QUERY 选定导出表子集的子句
* **下列关键字仅用于可传输的表空间**
* TRANSPORT_TABLESPACE 导出可传输的表空间元数据 (N)
* TABLESPACES 将传输的表空间列表

###IMP参数详解
使用的格式是：IMP KEYWORD=value 或 KEYWORD=(value1,value2,…,valueN) 
其中USERID是必须的且为第一个参数

**关键字 备注**
* USERID 用户名and口令
* FULL 导入整个文件(N)
* BUFFER 数据缓冲区大小
* FROMUSER 所有者用户名列表
* TOUSER 用户名列表
* FILE 输入文件 (EXPDAT.DMP)
* SHOW 只列出文件内容(N)
* TABLES 表名列表
* IGNORE 忽略创建错误 (N)
* RECORDLENGTH IO 记录的长度
* GRANTS 导入权限 (Y)
* INCTYPE 增量导入类型
* INDEXES 导入索引 (Y)
* COMMIT 提交数组插入 (N)
* ROWS 导入数据行 (Y)
* PARFILE 参数文件名
* LOG 屏幕输出的日志文件
* CONSTRAINTS 导入限制 (Y)
* DESTROY 覆盖表空间数据文件 (N)
* INDEXFILE 将表and索引信息写入指定的文件
* SKIP_UNUSABLE_INDEXES 跳过不可用索引的维护 (N)
* FEEDBACK 每 x 行显示进度 (0)
* TOID_NOVALIDATE 跳过指定类型 ID 的验证
* FILESIZE 每个转储文件的最大大小
* STATISTICS 始终导入预计算的统计信息
* RESUMABLE 在遇到有关空间的错误时挂起 (N)
* RESUMABLE_NAME 用来标识可恢复语句的文本字符串
* RESUMABLE_TIMEOUT RESUMABLE 的等待时间
* COMPILE 编译过程, 程序包和函数 (Y)
* STREAMS_CONFIGURATION 导入流的一般元数据 (Y)
* STREAMS_INSTANTIATION 导入流实例化元数据 (N)
* **下列关键字仅用于可传输的表空间**
* TRANSPORT_TABLESPACE 导入可传输的表空间元数据 (N)
* TABLESPACES 将要传输到数据库的表空间
* DATAFILES 将要传输到数据库的数据文件
* TTS_OWNERS 拥有可传输表空间集中数据的用户