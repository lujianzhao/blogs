## 一、环境
[`hadoop-2.7.3.tar.gz`](http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz)<br>
[`jdk-8u121-linux-x64.tar.gz`](http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.tar.gz)<br>
[`CentOS-7-x86_64-DVD-1511.iso`](http://vault.centos.org/7.2.1511/isos/x86_64/CentOS-7-x86_64-DVD-1511.iso)<br>
`VMware Workstation 11.0`、`xshell`、`xftp`<br>
使用VMware虚拟4台centos，结构如下：<br>
`master`  `192.168.8.8`<br>
`slave1`  `192.168.8.10`<br>
`slave2`  `192.168.8.11`<br>
`slave3`  `192.168.8.12`<br>

## 二、环境搭建
### 配置网络
```bash
[root@master ~]# vim /etc/sysconfig/network-scripts/ifcfg-eno16777736 
TYPE=Ethernet
IPADDR=192.168.8.8
NETMASK=255.255.255.0

[root@master ~]# vim /etc/sysconfig/network
# Created by anaconda
# NETWORKING=YES
# GATEWAY=192.168.8.1
```

### 禁用防火墙
```bash
[root@master ~]# systemctl disable firewalld
```

### 配置主机名
```bash
[root@master ~]# hostnamectl set-hostname master
```

### 解压hadoop,jdk配置环境变量
```bash
[root@master local]# tar -zxvf hadoop-2.7.3.tar.gz
[root@master local]# tar -zxvf jdk-8u121-linux-x64.tar.gz

[root@master ~]# vim /etc/profile
export JAVA_HOME=/usr/local/jdk
export CLASSPATH=.:$JAVA_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin
export PATH=$PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin
[root@master ~]# source /etc/profile
```

### 配置hadoop
```bash
[root@master hadoop]# vim core-site.xml
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://master:9000</value>
        </property>
</configuration>

[root@master hadoop]# vim hadoop-env.sh
export JAVA_HOME=/usr/local/jdk
```

### NameNode
```bash
# 格式化namenode
[root@master ~]# hdfs namenode -format
# 运行namenode
[root@master ~]# hadoop-daemon.sh start namenode
starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-master.out
# 查看java进程
[root@master ~]# jps
3078 NameNode
3146 Jps
# namenode起不来，需要看日志
[root@master logs]# more hadoop-root-namenode-master.log
```
**重要的信息如下：**
```bash
org.apache.hadoop.hdfs.server.common.InconsistentFSStateException: Directory /tmp/hadoop-root
/dfs/name is in an inconsistent state: storage directory does not exist or is not accessible.
```
**`name`目录不存在，创建即可，创建后需要`重新格式化`才能起来**

### DataNode
```bash
# 运行datanode
[root@slave1 hadoop]# hadoop-daemon.sh start datanode
starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-slave1.out
# 查看java进程
[root@slave1 hadoop]# jps
2472 DataNode
2504 Jps
# datanode起不来，需要查看日志，原因为namenode和datanode的clusterID不相同
java.io.IOException: Incompatible clusterIDs in /tmp/hadoop-root/dfs/data: 
namenode clusterID = CID-e029cb6c-ef43-4a34-97a1-7ab1d2216243; 
datanode clusterID = CID-9c0d9da2-fec7-4c38-80db-1c72df152578
java.io.IOException: All specified directories are failed to load.
```

### 修改DataNode的clusterID
```bash
[root@master current]# more /tmp/hadoop-root/dfs/name/current/VERSION 
#Fri Mar 24 23:03:40 CST 2017
namespaceID=1358526885
clusterID=CID-9c0d9da2-fec7-4c38-80db-1c72df152578
cTime=0
storageType=NAME_NODE
blockpoolID=BP-1764672276-192.168.8.8-1490367820827
layoutVersion=-63

[root@slave1 current]# more /tmp/hadoop-root/dfs/data/current/VERSION 
#Fri Mar 24 23:44:08 CST 2017
storageID=DS-acfdf091-fb05-440b-9dcf-9b2736987b39
clusterID=CID-9c0d9da2-fec7-4c38-80db-1c72df152578
cTime=0
datanodeUuid=ecd96d82-72c9-4216-88c3-f70d0bb442c9
storageType=DATA_NODE
layoutVersion=-56
```
http://stackoverflow.com/questions/30521474/hadoop-hdfs-formatting-gets-error-failed-for-block-pool

### web管理界面
访问`master`地址`http://192.168.8.8:50070/` 即可