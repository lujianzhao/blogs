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