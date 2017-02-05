###一、问题：
1. 局域网写代码连`Oracle`很慢，`SELECT * FROM DUAL` 要执行8分钟；
2. `PL/SQL`连`Oracle`假死很久才连上，连上后打开`SQL WINDOW`，执行`SQL`，初始化很慢，执行却不慢；
3. 应用起不来，后来才发现能起来，不过要10多分钟；

###二、解决方法：
**问题产生的主要原因是oracle数据库的监听日志过大（4G）导致，​删除日志文件即可恢复正常。**
文件位置：
```bash
D:\app\serical\diag\tnslsnr\DESKTOP-SCE4JAT\listener\trace\listener.log
```

1、停止监听器
```bash
C:\windows\system32>lsnrctl stop

LSNRCTL for 64-bit Windows: Version 11.2.0.1.0 - Production on 05-2月 -2017 16:31:49

Copyright (c) 1991, 2010, Oracle.  All rights reserved.

正在连接到 (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
命令执行成功
```

2、删除`listener.log`文件

3、启动监听器
```bash
C:\windows\system32>lsnrctl start

LSNRCTL for 64-bit Windows: Version 11.2.0.1.0 - Production on 05-2月 -2017 16:31:59

Copyright (c) 1991, 2010, Oracle.  All rights reserved.

启动tnslsnr: 请稍候...

TNSLSNR for 64-bit Windows: Version 11.2.0.1.0 - Production
系统参数文件为D:\app\serical\product\11.2.0\dbhome_1\NETWORK\ADMIN\listener.ora
写入d:\app\serical\diag\tnslsnr\DESKTOP-SCE4JAT\listener\alert\log.xml的日志信息
监听: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(PIPENAME=\\.\pipe\EXTPROC1521ipc)))
监听: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=DESKTOP-SCE4JAT)(PORT=1521)))

正在连接到 (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
LISTENER 的 STATUS
------------------------
别名                      LISTENER
版本                      TNSLSNR for 64-bit Windows: Version 11.2.0.1.0 - Production
启动日期                  05-2月 -2017 16:32:01
正常运行时间              0 天 0 小时 0 分 1 秒
跟踪级别                  off
安全性                    ON: Local OS Authentication
SNMP                      OFF
监听程序参数文件          D:\app\serical\product\11.2.0\dbhome_1\NETWORK\ADMIN\listener.ora
监听程序日志文件          d:\app\serical\diag\tnslsnr\DESKTOP-SCE4JAT\listener\alert\log.xml
监听端点概要...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(PIPENAME=\\.\pipe\EXTPROC1521ipc)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=DESKTOP-SCE4JAT)(PORT=1521)))
服务摘要..
服务 "CLRExtProc" 包含 1 个实例。
  实例 "CLRExtProc", 状态 UNKNOWN, 包含此服务的 1 个处理程序...
命令执行成功
```