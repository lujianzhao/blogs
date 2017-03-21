### 安装VMTools工具
```bash
#虚拟机-->安装Vmware Tools
[root@serical01 /]# cp VMwareTools-8.6.1-19175.tar.gz ~/Downloads
[root@serical01 /]# tar -zxvf VMwareTools-8.6.1-19175.tar.gz
[root@serical01 /]# cd vmware-tools-distrib
[root@serical01 /]# ./vmware-install.pl
#重启
[root@serical01 /]# reboot
```

### 配置IP
```bash
[root@serical01 /]# vim /etc/sysconfig/network-scripts/ifcfg-eth0
#主要修改如下：
BOOTPROTO=static
IPADDR=192.168.8.88
NETMASK=255.255.255.0
GATEWAY=192.168.8.1
DNS1=8.8.8.8
DNS1=8.8.4.4
```

### 配置主机名
```bash
[root@serical01 /]# vim /etc/sysconfig/network
HOSTNAME=serical01

[root@serical01 /]# vim /etc/hosts
192.168.8.88    serical01
```

### 关闭防火墙
```bash
#查看各种启动级别下iptables自启动情况
[root@serical01 /]# chkconfig iptables --list
#7种启动级别查看
[root@serical01 /]# vim /etc/inittab
#关闭防火墙自启动
[root@serical01 /]# chkconfig iptables off
```

### 配置163的yum源
```bash
#1.下载repo文件
[root@serical01 /]# wget http://mirrors.163.com/.help/CentOS6-Base-163.repo
#2.备份并替换系统的repo文件
[root@serical01 /]# cd /etc/yum.repos.d/
[root@serical01 /]# mv CentOS-Base.repo CentOS-Base.repo.bak
[root@serical01 /]# mv CentOS6-Base-163.repo CentOS-Base.repo
#3.执行yum源更新
[root@serical01 /]# yum clean all
[root@serical01 /]# yum makecache
[root@serical01 /]# yum update
```