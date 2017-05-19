### 1. Install and configure the necessary dependencies
[ Download GitLab Community Edition (CE) | GitLab ](https://about.gitlab.com/downloads/#centos7)<br>
If you install Postfix to send email please select 'Internet Site' during setup. Instead of using Postfix you can also use Sendmail or [configure a custom SMTP server](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/smtp.md) and [configure it as an SMTP server](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/smtp.md#smtp-on-localhost).

On CentOS, the commands below will also open HTTP and SSH access in the system firewall.
```bash
sudo yum install curl policycoreutils openssh-server openssh-clients
sudo systemctl enable sshd
sudo systemctl start sshd
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```

### 2. Add the GitLab package server and install the package
```bash
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo yum install gitlab-ce
```
If you are not comfortable installing the repository through a piped script, you can find the [entire script here](https://packages.gitlab.com/gitlab/gitlab-ce/install) and [select and download the package manually](https://packages.gitlab.com/gitlab/gitlab-ce) and install using
```bash
curl -LJO https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-XXX.rpm/download
rpm -i gitlab-ce-XXX.rpm
```
手动下载`rpm`包，根据上面的地址和版本地址如下，如果下载慢可以用国外VPS下载再复制回来。<br>
https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-9.1.4-ce.0.el7.x86_64.rpm/download

### 3. Configure and start GitLab
```bash
sudo gitlab-ctl reconfigure
```

### 4. Browse to the hostname and login

On your first visit, you'll be redirected to a password reset screen to provide the password for the initial administrator account. Enter your desired password and you'll be redirected back to the login screen.
The default account's username is `root`. Provide the password you created earlier and login. After login you can change the username if you wish.

 For configuration and troubleshooting options please see the [Omnibus GitLab documentation](http://doc.gitlab.com/omnibus/)<br>
 If you are located in China, try using https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/

### 5. Gitlab uninstall
 ```bash
 # 删除gitlab
 sudo gitlab-ctl uninstall
 sudo rpm -e gitlab-ce

# 重装需要删除如下目录
[root@localhost ~]# find / -name gitlab

# ruby_block[supervise_redis_sleep] action run卡住
[root@localhost ~]# systemctl restart gitlab-runsvdir
# 再次执行
[root@localhost ~]# gitlab-ctl reconfigure
 ```