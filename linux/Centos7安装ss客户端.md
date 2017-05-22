[参考文档](https://www.linuxbabe.com/desktop-linux/how-to-install-and-use-shadowsocks-command-line-client)
### Install the Command Line Client
```bash
sudo yum install python-setuptools
sudo easy_install pip
sudo pip install shadowsocks
```

### Create a Configuration File
we will create a configuration file under /etc/
```bash
 sudo vi /etc/shadowsocks.json
```
Put the following text in the file. Replace `server-ip` with your actual IP and set a `password`.
```bash
{
"server":"server-ip",
"server_port":8000,
"local_address": "127.0.0.1",
"local_port":1080,
"password":"your-password",
"timeout":600,
"method":"aes-256-cfb"
}
```
Save and close the file. Next start the client using command line
```bash
sslocal -c /etc/shadowsocks.json
```
### To run in the background
```bash
sudo sslocal -c /etc/shadowsocks.json -d start
```

### Auto Start the Client on System Boot
Edit `/etc/rc.local` file
```bash
sudo vi /etc/rc.local
````
Put the following line above the exit 0 line:
```bash
sudo sslocal -c /etc/shadowsocks.json -d start
```
Save and close the file. Next time you start your computer, shadowsocks client will automatically start and connect to your shadowsocks server.

### Check if It Works

After you rebooted your computer, enter the following command in terminal:
```bash
sudo systemctl status rc-local.service
```
If your sslocal command works then you will get this ouput:
```bash
● rc-local.service - /etc/rc.local Compatibility
Loaded: loaded (/etc/systemd/system/rc-local.service; enabled; vendor preset: enabled)
Active: active (running) since Fri 2015-11-27 03:19:25 CST; 2min 39s ago
Process: 881 ExecStart=/etc/rc.local start (code=exited, status=0/SUCCESS)
CGroup: /system.slice/rc-local.service
├─ 887 watch -n 60 su matrix -c ibam
└─1112 /usr/bin/python /usr/local/bin/sslocal -c /etc/shadowsocks....
```
As you can see from the last line, the sslocal command created a process whose pid is 1112 on my machine. It means shadowsocks client is running smoothly. And of course you can tell your browser to connect through your shadowsocks client to see if everything goes well.

If for some reason your `/etc/rc.local` script won’t run, then check the following post to find the solution.
[How to enable /etc/rc.local with Systemd](https://www.linuxbabe.com/linux-server/how-to-enable-etcrc-local-with-systemd)