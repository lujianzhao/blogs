###使用方式 : chmod [-cfvR] [--help] [--version] mode file...
###说明 :
Linux/Unix 的档案存取权限分为三级 : 档案拥有者、群组、其他。利用 chmod 可以藉以控制档案如何被他人所存取。 mode : 权限设定字串，格式如下 : [ugoa...][[+-=][rwxX]...][,...]，其中: 
* u 表示该档案的拥有者， 
* g 表示与该档案的拥有者属于同一个群体(group)者， 
* o 表示其他以外的人， 
* a 表示这三者皆是。 
* + 表示增加权限、 
* - 表示取消权限、 
* = 表示唯一设定权限。 
* r 表示可读取， 
* w 表示可写入， 
* x 表示可执行， 
* X 表示只有当该档案是个子目录或者该档案已经被设定过为可执行。 
* -c : 若该档案权限确实已经更改，才显示其更改动作 
* -f : 若该档案权限无法被更改也不要显示错误讯息 
* -v : 显示权限变更的详细资料 
* -R : 对目前目录下的所有档案与子目录进行相同的权限变更(即以递回的方式逐个变更) 
* –help : 显示辅助说明 
* –version : 显示版本

###使用例子
```bash
[root@serical01 test]# ll -R
.:
total 8
-rw-r--r--. 1 root root   14 Jul  8 20:58 a
drwxr-xr-x. 2 root root 4096 Jul  8 21:01 t
./t:
total 4
-rw-r--r--. 1 root root 40 Jul  8 21:01 b
```

###给当前路径所有文件及目录递归的拥有者加上x（执行）权限
```bash
[root@serical01 test]# chmod u+x -R *
[root@serical01 test]# ll -R
.:
total 8
-rwxr--r--. 1 root root   14 Jul  8 20:58 a
drwxr-xr-x. 2 root root 4096 Jul  8 21:01 t
./t:
total 4
-rwxr--r--. 1 root root 40 Jul  8 21:01 b
```

###拥有者去掉r权限，所属组加上w权限，其他赋予rwx权限
```bash
[root@serical01 test]# chmod u-r,g+w,o=rwx -R *
[root@serical01 test]# ll -R
.:
total 8
--wxrw-rwx. 1 root root   14 Jul  8 20:58 a
d-wxrwxrwx. 2 root root 4096 Jul  8 21:01 t
./t:
total 4
--wxrw-rwx. 1 root root 40 Jul  8 21:01 b
```

###数字表示形式r（4）w（2）x（1），表示u=rwx,g=rx,o=rx
```bash
[root@serical01 test]# chmod 755 -R *
[root@serical01 test]# ll -R
.:
total 8
-rwxr-xr-x. 1 root root   14 Jul  8 20:58 a
drwxr-xr-x. 2 root root 4096 Jul  8 21:01 t
./t:
total 4
-rwxr-xr-x. 1 root root 40 Jul  8 21:01 b
```