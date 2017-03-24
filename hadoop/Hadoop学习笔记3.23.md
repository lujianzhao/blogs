## 配置集群启动
### 配置slaves
```
[root@master hadoop]# pwd
/usr/local/hadoop/etc/hadoop
[root@master hadoop]# vim slaves 
slave1
slave2
slave3
```

### 配置ssh免登录
```
[root@master hadoop]# start-dfs.sh 
Starting namenodes on [master]
The authenticity of host 'master (192.168.8.8)' can't be established.
ECDSA key fingerprint is 9f:ba:77:ff:03:c0:2b:10:fc:c5:df:0d:63:20:28:43.
Are you sure you want to continue connecting (yes/no)? yes
master: Warning: Permanently added 'master,192.168.8.8' (ECDSA) to the list of known hosts.
root@master's password: 
master: namenode running as process 2364. Stop it first.
The authenticity of host 'slave2 (192.168.8.11)' can't be established.
ECDSA key fingerprint is 9f:ba:77:ff:03:c0:2b:10:fc:c5:df:0d:63:20:28:43.
Are you sure you want to continue connecting (yes/no)? The authenticity of host 'slave3 (192.168.8.12)' can't be established.
ECDSA key fingerprint is 9f:ba:77:ff:03:c0:2b:10:fc:c5:df:0d:63:20:28:43.
Are you sure you want to continue connecting (yes/no)? The authenticity of host 'slave1 (192.168.8.10)' can't be established.
ECDSA key fingerprint is 9f:ba:77:ff:03:c0:2b:10:fc:c5:df:0d:63:20:28:43.
Are you sure you want to continue connecting (yes/no)? yes
slave2: Warning: Permanently added 'slave2,192.168.8.11' (ECDSA) to the list of known hosts.
root@slave2's password: Please type 'yes' or 'no': 
slave3: Warning: Permanently added 'slave3,192.168.8.12' (ECDSA) to the list of known hosts.
root@slave3's password: Please type 'yes' or 'no': 
slave1: Warning: Permanently added 'slave1,192.168.8.10' (ECDSA) to the list of known hosts.
root@slave1's password: 
slave2: datanode running as process 2379. Stop it first.
# 可以看到每台机器都需要输入密码，很麻烦

# 生成秘钥
[root@master hadoop]# ssh-keygen -t rsa
[root@master hadoop]# cd ~/.ssh
[root@master .ssh]# ls
id_rsa  id_rsa.pub  known_hosts

# 复制公钥到各个slave节点
[root@master .ssh]# ssh-copy-id slave1
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@slave1's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'slave1'"
and check to make sure that only the key(s) you wanted were added.

[root@master .ssh]# ssh-copy-id slave2
[root@master .ssh]# ssh-copy-id slave3
# 注意master自己也需要添加
[root@master .ssh]# ssh-copy-id  master

# 已经复制到各个节点
[root@slave1 ~]# ll .ssh/
总用量 4
-rw-------. 1 root root 393 3月  25 00:07 authorized_keys
```

### 集群管理
```
# 启动集群
[root@master ~]# start-dfs.sh 
Starting namenodes on [master]
master: starting namenode, logging to /usr/local/hadoop/logs/hadoop-root-namenode-master.out
slave2: starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-slave2.out
slave3: starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-slave3.out
slave1: starting datanode, logging to /usr/local/hadoop/logs/hadoop-root-datanode-slave1.out
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/hadoop/logs/hadoop-root-secondarynamenode-master.out
[root@master ~]# jps
3865 Jps
3755 SecondaryNameNode
3565 NameNode

# 停止集群
[root@master ~]# stop-dfs.sh 
Stopping namenodes on [master]
master: stopping namenode
slave1: stopping datanode
slave2: stopping datanode
slave3: stopping datanode
Stopping secondary namenodes [0.0.0.0]
0.0.0.0: stopping secondarynamenode
[root@master ~]# jps
4184 Jps
```