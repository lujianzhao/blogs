###备份
查找redis安装目录，即备份文件目录
```bash
127.0.0.1:6379> config get dir
1) "dir"
2) "/var/lib/redis"
```

手动执行save命令
```bash
127.0.0.1:6379> save
OK
```

可以看到第一步目录下有个dump.rdb
```bash
[root@localhost redis]# ls
dump.rdb
```

###恢复
然后就发现了大问题，`备份`服务器上的只采用了`RDB`备份方式没采用`AOF`方式，但`恢复`服务器则采用了
`RDB+AOF`方式，恢复的时候发现是`AOF`方式优先，但由于备份并没有采用`AOF`方式导致没有
`appendonly.aof`备份文件，只能通过`RDB`方式恢复了

1. 第一次恢复
把`dump.rdb`复制到恢复服务器根目录，然后重启`Redis`，然后查看并没有恢复，原因是`appendonly yes`。

2. 第二次恢复
关闭`AOF`即`appendonly no`，然后重启`Redis`，然后查看并没有恢复，原因是第一次重启后`dump.rdb`被覆盖了。

3. 第三次恢复
重新把`dump.rdb`复制到恢复服务器根目录，然后重启`Redis`，数据已经恢复。
重新开启RDB+AOF备份方案（appendonly yes）
重启`Redis`后发现，刚刚恢复好的数据又没了`:(`，原因是优先`AOF`恢复，而`appendonly.aof`文件并没有改
变，所以`Redis`又恢复到了以前的状态。

4. 回到第三次恢复状态
关闭`AOF`即`appendonly no`，重新把`dump.rdb`复制到恢复服务器根目录，然后重启`Redis`，数据已经恢复。

重写appendonly.aof为当前数据的备份
```bash
127.0.0.1:6379> bgrewriteaof
Background append only file rewriting started
```
**最后严重警告：千万不要在只开启了RDB方式途中开启AOF方式，不然数据库会被清空，因为开启AOF时appendonly.aof是空的，优先AOF恢复，所以就被清空啦。我是在恢复好后，准备备份服务器也开启RDB+AOF方式，然后就悲剧了，不过还好采用AOF方式恢复了。
PS：AOF恢复就是把appendonly.aof复制到config get dir目录下，重启数据库就OK啦。**