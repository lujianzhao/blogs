###一、原因
由于ACE产品即将下架，配套MYSQL服务也即将关闭，导致必须要把数据库迁移到云服务器上。

###二、迁移过程
**Before You Begin**<br>
Ensure that you have followed the Getting Started and Securing Your Server guides, and the Linode’s hostname is set.
To check your hostname run:
```bash
hostname
hostname -f
```

The first command should show your short hostname, and the second should show your fully qualified domain name (FQDN).
Update your system:
```bash
sudo yum update
```

**Install MySQL**<br>
MySQL must be installed from the community repository.
Download and add the repository, then update.
```bash
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum update
```

Install MySQL as usual and start the service. During installation, you will be asked if you want to accept the results from 
the .rpm file’s GPG verification. If no error or mismatch occurs, enter y.
```bash
sudo yum install mysql-server
sudo systemctl start mysqld
sudo systemctl enable mysqld
```
MySQL will bind to localhost (127.0.0.1) by default. Please reference our MySQL remote access guide for information on 
connecting to your databases using SSH.

Allowing unrestricted access to MySQL on a public IP not advised but you may change the address it listens on by modifying the 
bind-address parameter in /etc/my.cnf. If you decide to bind MySQL to your public IP, you should implement firewall rules 
that only allow connections from specific IP addresses.

**Harden MySQL Server**<br>
Run the `mysql_secure_installation` script to address several security concerns in a default MySQL installation.
```bash
sudo mysql_secure_installation
```
You will be given the choice to change the MySQL root password, remove anonymous user accounts, disable root logins 
outside of localhost, and remove test databases. It is recommended that you answer yes to these options. 
You can read more about the script in in the MySQL Reference Manual.

**Using MySQL**<br>
The standard tool for interacting with MySQL is the mysql client which installs with the mysql-server package. 
The MySQL client is used through a terminal.

Root Login
To log in to MySQL as the root user:
```bash
mysql -u root -p
```
When prompted, enter the root password you assigned when the mysql_secure_installation script was run.

You’ll then be presented with a welcome header and the MySQL prompt as shown below:
```bash
mysql>
```

**Create a New MySQL User and Database**<br>
In the example below, testdb is the name of the database, testuser is the user, and password is the user’s password.
```bash
create database testdb;
create user 'testuser'@'localhost' identified by 'password';
grant all on testdb.* to 'testuser' identified by 'password';
```
You can shorten this process by creating the user while assigning database permissions:
```bash
create database testdb;
grant all on testdb.* to 'testuser' identified by 'password';
```
if you want to internet access mysql
```bash
update user set host='%' where user='testuser';
systemctl restart mysqld;
```

###乱码
```bash
#创建数据库时指定编码
CREATE DATABASE mydb
  DEFAULT CHARACTER SET utf8
  DEFAULT COLLATE utf8_general_ci;
[mysqld]
collation_server=utf8_general_ci
character_set_server=utf8
```

###mysql导出导入
```bash
// 导出
mysqldump -uroot -ppasswd dbname > p.sql
// 导入
mysql -uroot -ppasswd dbname < d:/p.sql
```

###sublime换行
```js
// Preferences->Setting-User:
{
    "color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
    "font_size": 16,
    "ignored_packages":
    [
        "Vintage"
    ],
    "word_wrap":true
}
```