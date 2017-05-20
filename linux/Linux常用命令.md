
### 修改IP
```bash
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-eno16777736
TYPE=Ethernet
IPADDR=192.168.1.67
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

### 查找directory
```bash
[root@localhost ~]# find / -type d -name gitlab
/etc/gitlab
/var/log/gitlab
/var/opt/gitlab

# 同上面一样
[root@localhost ~]# find / -name gitlab
/etc/gitlab
/var/log/gitlab
/var/opt/gitlab

# 查找文件
[root@localhost ~]# find / -name *gitlab*
/root/gitlab-ce-9.1.4-ce.0.el7.x86_64.rpm

# 删除find结果
[root@localhost ~]# find / -name gitlab|xargs rm -rf
```


### 软连接和硬连接
```bash
[root@localhost test]# touch a && echo this is a txt file > a
[root@localhost test]# cat a
this is a txt file
# 创建硬链接
[root@localhost test]# ln a b
# 创建软连接
[root@localhost test]# ln -s a c

# 查看
[root@localhost test]# ll -li
总用量 8
607569 -rw-r--r--. 2 root root 19 5月  20 11:46 a
607569 -rw-r--r--. 2 root root 19 5月  20 11:46 b
607570 lrwxrwxrwx. 1 root root  1 5月  20 11:46 c -> a

# vim b c都能对a修改，主要区别在于删除上
[root@localhost test]# rm -f a
[root@localhost test]# ls -li
总用量 4
607569 -rw-r--r--. 1 root root 19 5月  20 11:46 b
607570 lrwxrwxrwx. 1 root root  1 5月  20 11:46 c -> a
[root@localhost test]# cat a
cat: a: 没有那个文件或目录
[root@localhost test]# cat b
this is a txt file
[root@localhost test]# cat c
cat: c: 没有那个文件或目录

# 再去创建a或者c文件都能重新生成a文件，但是b和a已经不是硬链接的关系了
[root@localhost test]# echo another > c
[root@localhost test]# cat a
another
[root@localhost test]# cat b
this is a txt file
[root@localhost test]# cat c
another
```