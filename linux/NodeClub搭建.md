[Git地址](https://github.com/cnodejs/nodeclub/)
### 安装教程
```bash
1. 安装 `Node.js[必须]` `MongoDB[必须]` `Redis[必须]`
2. 启动 MongoDB 和 Redis
3. `$ make install` 安装 Nodeclub 的依赖包
4. `cp config.default.js config.js` 请根据需要修改配置文件
5. `$ make test` 确保各项服务都正常
6. `$ node app.js`
7. visit `http://localhost:3000`
8. done!
```
**注意不要在`windows`下`make install` 复制到`centos`上，不然`make build`出现权限问题**

### 邮箱配置
```bash
// 邮箱配置
  mail_opts: {
    host: 'smtp.qq.com',
    port: 465,
    secureConnection: true,
    auth: {
      user: 'serical_net@qq.com',
      pass: 'xxx' // 开启QQ邮箱SMTP时的授权码，不是QQ密码，更不是独立密码
    },
    ignoreTLS: true
  }
```

### 开机启动
```bash
# 脚本
[root@localhost ~]# vim /etc/rc.local
export PATH=/usr/local/nodejs/bin
forever /root/source/nodeclub/app.js

# 增加执行权限
[root@localhost ~]# ll /etc/rc.d/rc.local
-rw-r--r--. 1 root root 512 5月  23 14:01 /etc/rc.d/rc.local
[root@localhost ~]# chmod +x /etc/rc.d/rc.local
[root@localhost ~]# ll /etc/rc.d/rc.local
-rwxr-xr-x. 1 root root 512 5月  23 14:01 /etc/rc.d/rc.local

# 错误排查
[root@localhost ~]# cat /var/log/boot.log
[FAILED] Failed to start /etc/rc.d/rc.local Compatibility.
See 'systemctl status rc-local.service' for details.
         Starting Terminate Plymouth Boot Screen...
         Starting Wait for Plymouth Boot Screen to Quit...

# 使用systemctl status rc-local.service
[root@localhost ~]# systemctl status rc-local.service
● rc-local.service - /etc/rc.d/rc.local Compatibility
   Loaded: loaded (/usr/lib/systemd/system/rc-local.service; static; vendor preset: disabled)
   Active: failed (Result: exit-code) since 二 2017-05-23 14:12:06 CST; 1min 1s ago
  Process: 1157 ExecStart=/etc/rc.d/rc.local start (code=exited, status=127)

5月 23 14:12:06 localhost.localdomain systemd[1]: Starting /etc/rc.d/rc.local Compatibility...
5月 23 14:12:06 localhost.localdomain rc.local[1157]: /etc/rc.d/rc.local:行15: forever: 未找到命令
5月 23 14:12:06 localhost.localdomain systemd[1]: rc-local.service: control process exited, code=exited status=127
5月 23 14:12:06 localhost.localdomain systemd[1]: Failed to start /etc/rc.d/rc.local Compatibility.
5月 23 14:12:06 localhost.localdomain systemd[1]: Unit rc-local.service entered failed state.
5月 23 14:12:06 localhost.localdomain systemd[1]: rc-local.service failed.

# 增加forever环境变量export PATH=/usr/local/nodejs/bin，即启动成功
[root@localhost ~]# systemctl status rc-local.service
● rc-local.service - /etc/rc.d/rc.local Compatibility
   Loaded: loaded (/usr/lib/systemd/system/rc-local.service; static; vendor preset: disabled)
   Active: activating (start) since 二 2017-05-23 14:23:36 CST; 5min ago
  Control: 1162 (rc.local)
   CGroup: /system.slice/rc-local.service
           ├─1162 /bin/bash /etc/rc.d/rc.local start
           ├─1220 node /usr/local/nodejs/bin/forever /root/source/nodeclub/app.js
           └─2388 /usr/local/nodejs/bin/node /root/source/nodeclub/app.js

5月 23 14:23:36 localhost.localdomain systemd[1]: Starting /etc/rc.d/rc.local Compatibility...
5月 23 14:23:42 localhost.localdomain rc.local[1162]: warn:    --minUptime not set. Defaulting to: 1000ms
5月 23 14:23:42 localhost.localdomain rc.local[1162]: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
5月 23 14:24:13 localhost.localdomain rc.local[1162]: log4js.fileAppender - Writing to file logs/cheese.log, error happened  { [Error: ENOENT: no such file or directory, open...heese.log']
5月 23 14:24:13 localhost.localdomain rc.local[1162]: errno: -2,
5月 23 14:24:13 localhost.localdomain rc.local[1162]: code: 'ENOENT',
5月 23 14:24:13 localhost.localdomain rc.local[1162]: syscall: 'open',
5月 23 14:24:13 localhost.localdomain rc.local[1162]: path: 'logs/cheese.log' }
Hint: Some lines were ellipsized, use -l to show in full.
```