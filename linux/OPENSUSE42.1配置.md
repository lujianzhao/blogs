### 换国内源
禁用官方源和DVD源
```bash
zypper -mr -d openSUSE-42.1-0
zypper -mr -d repo-oss
zypper -mr -d repo-update
zypper -mr -d repo-update-non-oss
```
### 切换为科大源 
https://lug.ustc.edu.cn/wiki/mirrors/help/opensuse
```bash
sudo zypper ar -fc https://mirrors.ustc.edu.cn/opensuse/distribution/leap/42.1/repo/oss USTC:42.1:OSS
sudo zypper ar -fc https://mirrors.ustc.edu.cn/opensuse/distribution/leap/42.1/repo/non-oss USTC:42.1:NON-OSS
sudo zypper ar -fc https://mirrors.ustc.edu.cn/opensuse/update/leap/42.1/oss USTC:42.1:UPDATE-OSS
sudo zypper ar -fc https://mirrors.ustc.edu.cn/opensuse/update/leap/42.1/non-oss USTC:42.1:UPDATE-NON-OSS
```
### 更新系统
```bash
sudo zypper update
```

### 切换JDK版本
由于`suse`自带安装`openjdk`，删除比较麻烦，这里选择切换JDK版本，JDK安装和普通linux一样，解压然后修改`/etc/profile`,
然后需要`update-alternatives`命令，安装`java`,`javac`

安装java
```bash
update-alternatives --install /usr/bin/java java /usr/java/jdk1.7.0_67/bin/java 90
#安装javac
update-alternatives --install /usr/bin/javac javac /usr/java/jdk1.7.0_67/bin/javac 90
```
设置java命令版本
```bash
update-alternatives --config java
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                       Priority   Status
------------------------------------------------------------
* 0            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      auto mode
  1            /usr/java/jdk1.7.0_67/bin/java              90        manual mode
  2            /usr/lib64/jvm/jre-1.7.0-openjdk/bin/java   1705      manual mode
  3            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      manual mode

Press enter to keep the current choice[*], or type selection number: 
```
这里有几个版本选择，默认选中的是第一个，可以看到是openjdk，查看链接，最终指向的是openjdk的java命令
```bash
ll /usr/bin/java
lrwxrwxrwx 1 root root 22 11月 28 21:52 /usr/bin/java -> /etc/alternatives/java
 ll /etc/alternatives/java
lrwxrwxrwx 1 root root 41 12月  7 16:01 /etc/alternatives/java -> /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java
```
这里选择第二个也就是Selection为1的
```bash
update-alternatives --config java
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                       Priority   Status
------------------------------------------------------------
  0            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      auto mode
* 1            /usr/java/jdk1.7.0_67/bin/java              90        manual mode
  2            /usr/lib64/jvm/jre-1.7.0-openjdk/bin/java   1705      manual mode
  3            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      manual mode

Press enter to keep the current choice[*], or type selection number: 
```
再查看链接，可以看到最终指向了自己安装的jdk
```bash
ll /usr/bin/java
lrwxrwxrwx 1 root root 22 11月 28 21:52 /usr/bin/java -> /etc/alternatives/java
ll /etc/alternatives/java
lrwxrwxrwx 1 root root 30 12月  7 16:10 /etc/alternatives/java -> /usr/java/jdk1.7.0_67/bin/java
```

### SQL DEVELOPER
图标启动不了，修改
```bash
#SetJavaHome，为有效JDK路径即可
vim .sqldeveloper/4.1.0/product.conf 
```