###Win10企业版激活，管理员运行CMD，然后执行以下命令
```bash
slmgr.vbs /upk
slmgr /ipk NPPR9-FWDCX-D2C8J-H872K-2YT43
slmgr /skms zh.us.to
slmgr /ato
```

###修改窗口颜色：
```bash
HKEY_CURRENT_USER\Control Panel\Colors
Window 改为 204 232 207
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\DefaultColors\Standard
Window的值为 `cce8cf` 其实就是 204 232 207的16进制数
```
![颜色图片](https://dn-serical.qbox.me/1.png)

###删除自带应用
```bash
Get-AppxPackage -AllUsers | Remove-AppxPackage
```

###Win10离线安装.NET Framework 3.5
```bash
dism.exe /online /enable-feature /featurename:netfx3 /Source:L:\sources\sxs
```

###Centos7添加启动项代码
```bash
title CentOS7 
kernel (hd0,7)/isolinux/vmlinuz linux repo=hd:/dev/sda8:/
initrd (hd0,7)/isolinux/initrd.img
```