###PSFTP上传文件
```bash
D:\Program Files\putty>psftp
psftp: no hostname specified; use "open host.name" to connect
psftp> open root@192.168.8.88
Using username "root".
root@192.168.8.88's password:
Remote working directory is /root
psftp> put E:\Software\JDK\jdk-7u67-linux-x64.gz
local:E:\Software\JDK\jdk-7u67-linux-x64.gz => remote:/root/jdk-7u67-linux-x64.g
z
psftp> quit
```

###linux解压
```bash
#-C指定解压位置
[root@serical01 ~]# tar -zxvf jdk-7u67-linux-x64.gz -C /usr/java
```

###安装JDK
```bash
#查询centos自带的JDK
[root@serical01 bin]# rpm -qa|grep java
tzdata-java-2014g-1.el6.noarch
java-1.7.0-openjdk-1.7.0.65-2.5.1.2.el6_5.x86_64
java-1.6.0-openjdk-1.6.0.0-11.1.13.4.el6.x86_64
#卸载自带JDK
[root@serical01 bin]# rpm -e --nodeps tzdata-java-2014g-1.el6.noarch java-1.7.0-openjdk-1.7.0.65-2.5.1.2.el6_5.x86_64 java-1.6.0-openjdk-1.6.0.0-11.1.13.4.el6.x86_64
#添加环境变量
[root@serical01 jdk1.7.0_67]# vim /etc/profile
#G(文件结尾),o(换行插入),添加下面3行
export JAVA_HOME=/usr/java/jdk1.7.0_67
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib
#更新配置文件
[root@serical01 ~]# srouce /etc/profile
```