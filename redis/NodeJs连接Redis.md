### 允许局域网访问
修改配置文件`redis.conf`:
```bash
vim /etc/redis.conf 
#查找bind 127.0.0.1，将其注释掉
```

### 设置密码
修改配置文件`redis.conf`:
```bash
vim /etc/redis.conf 
#查找requirepass，将foobared改为自己的密码，重启redis
systemctl restart redis.service
```

### NodeJs连接Redis方式一
通过`createClient`方法第三个参数，`auth_pass`指定密码
```js
/**
 * Created by serical on 2015/11/30.
 */
var redis = require("redis");

REDIS_PORT = 6379;
REDIS_HOST = '192.168.254.117';
REDIS_PASS = 'xxx';
REDIS_OPTS = {auth_pass : REDIS_PASS};

client = redis.createClient(REDIS_PORT, REDIS_HOST, REDIS_OPTS);

client.on("error", function (err) {
   console.info('error : ' + err.stack);
});

client.on("ready", function (err) {
    console.info('log: ready');
});
```

### NodeJs连接Redis方式二
通过client的auth方法进行密码认证
```js
/**
 * Created by serical on 2015/11/30.
 */
var redis = require("redis");

REDIS_PORT = 6379;
REDIS_HOST = '192.168.254.117';
REDIS_PASS = 'xxx';
REDIS_OPTS = {};

client = redis.createClient(REDIS_PORT, REDIS_HOST, REDIS_OPTS);
client.auth(REDIS_PASS, function() {
   console.info('ok');
});
```