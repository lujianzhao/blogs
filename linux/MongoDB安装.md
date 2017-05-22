[官方文档](https://docs.mongodb.com/master/tutorial/install-mongodb-on-red-hat/)<br>
### Configure the package management system (yum).
Create a `/etc/yum.repos.d/mongodb-org-3.4.repo` file so that you can install MongoDB directly, using `yum`.
```bash
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc

# 国内
[mongodb-org]
name=MongoDB Repository
baseurl=http://mirrors.aliyun.com/mongodb/yum/redhat/7Server/mongodb-org/3.4/x86_64/
gpgcheck=0
enabled=1
```

### Install the MongoDB packages and associated tools.
To install the latest stable version of MongoDB, issue the following command:
```bash
# 安装
sudo yum install -y mongodb-org
# 启动
sudo service mongod start
# 开机自起
sudo chkconfig mongod on
# 停止
sudo service mongod stop
# 重启
sudo service mongod restart
```

### Uninstall MongoDB Community Edition
```bash
sudo service mongod stop
sudo yum erase $(rpm -qa | grep mongodb-org)
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongo
```