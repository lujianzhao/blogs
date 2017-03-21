### yum源
访问Nginx官网获取最新yum源 http://nginx.org/en/linux_packages.html

```bash
# 第一步(执行命令,安装nginx yum源)
rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
**如果要安装最新nginx,请修改/etc/yum.repos.d/nginx.repo文件**

### 例子:
```bash
# nginx.repo
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```