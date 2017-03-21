### 安装hadoop
```bash
[root@serical01 ~]# mkdir /serical
[root@serical01 ~]# tar -zxvf hadoop-2.2.0.tar.gz -C /serical
```

### 修改hadoop-env.sh
```bash
#修改/serical/hadoop-2.2.0/etc/hadoop目录下的hadoop-env.sh
[root@serical01 hadoop]# vim hadoop-env.sh
export JAVA_HOME=/usr/java/jdk1.7.0_67
```

### 修改core-site.xml
```xml
fs.defaultFS：用来指定HDFS的老大（NameNode）的地址 
hadoop.tmp.dir：用来指定hadood运行时产生文件的存放目录
#修改/serical/hadoop-2.2.0/etc/hadoop目录下的core-site.xml
[root@serical01 hadoop]# vim core-site.xml
<configuration>
        <property>
                <name>fs.defaultFS</name>
                <value>hdfs://serical01:9000</value>
        </property>
        <property>
                <name>hadoop.tmp.dir</name>
                <value>/serical/hadoop-2.2.0/tmp</value>
        </property>
</configuration>
```

### 修改hdfs-site.xml
```xml
dfs.replication：指定HDFS保存副本的数量
#修改/serical/hadoop-2.2.0/etc/hadoop目录下的hdfs-site.xml
[root@serical01 hadoop]# cat hdfs-site.xml
<configuration>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
</configuration>
```

### 修改mapred-site.xml
```xml
mapreduce.framework.name：告诉hadoop以后mp运行在yarn上
#复制一个模板文件
[root@serical01 hadoop]# cp mapred-site.xml.template  mapred-site.xml
##修改/serical/hadoop-2.2.0/etc/hadoop目录下的mapred-site.xml
[root@serical01 hadoop]# vim mapred-site.xml
<configuration>
        <prperty>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
</configuration>
```

### 修改yarn-site.xml
```xml
yarn.nodemanager.aux-services：NodeManager获取数据的方式是shuffle 
yarn.resourcemanager.hostname：指定YARN的老大（ResourceManager）的地址

#修改/serical/hadoop-2.2.0/etc/hadoop目录下的yarn-site.xml
[root@serical01 hadoop]# vim yarn-site.xml
<configuration>
<!-- Site specific YARN configuration properties -->
        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property> 
        <property>
                <name>yarn.resourcemanager.hostname</name>
                <value>serical01</value>
        </property>
</configuration>
```

### 配置hadoop环境变量
```bash
[root@serical01 hadoop]# vim /etc/profile
export JAVA_HOME=/usr/java/jdk1.7.0_67
export HADOOP_HOME=/serical/hadoop-2.2.0
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin                                                
[root@serical01 hadoop]# source /etc/profile
```

### 初始化HDFS（格式化文件系统）
```bash
#hadoop namenode -format（已过时）
#格式化文件系统
[root@serical01 bin]# hdfs namenode -format
```

### 启动
```bash
#./start-all.sh过时的
[root@serical01 sbin]# ./start-dfs.sh
[root@serical01 sbin]# ./start-yarn.sh
```
HDFS管理地址：[192.168.8.88:50070](http://192.168.8.88:50070)<br>
YARN管理地址：[192.168.8.88:8088](http://192.168.8.88:8088)

### 测试HDFS
```bash
#上传文件到hdfs
[root@serical01 serical]# hdfs fs -put ~/jdk-7u67-linux-x64.gz hdfs://serical01:9000/jdk
#下载hdfs文件
[root@serical01 serical]# hdfs fs -get hdfs://serical01:9000/jdk ~/jdk1.7
#Mapreduce的wordcount实例
[root@serical01 mapreduce]# hadoop jar hadoop-mapreduce-examples-2.2.0.jar wordcount hdfs://serical01:9000/words hdfs://serical01:9000/wcout
#ls命令
[root@serical01 mapreduce]# hadoop fs -ls hdfs://serical01:9000/
```

### SSH免登陆配置
```bash
#/root/.ssh目录下，生成秘钥
[root@serical01 .ssh]# ssh-keygen -t rsa
#将公钥添加到认证key上，即可免登录（authorized_keys不能写错）
[root@serical01 .ssh]# cp id_rsa.pub authorized_keys
#将192.168.8.88的公钥添加到192.168.8.89的authorized_keys上，即可实现88登录89免登录
[root@serical01 .ssh]# ssh-copy-id 192.168.8.89
```