```bash
# 修改IP
[root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-eno16777736
TYPE=Ethernet
IPADDR=192.168.1.67
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8

# 查找directory
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