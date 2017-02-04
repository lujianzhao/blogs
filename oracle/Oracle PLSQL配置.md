###一、增加环境变量
```bash
TNS_ADMIN = D:\app\serical\product\11.2.0\dbhome_1\NETWORK\ADMIN
NLS_LANG = SIMPLIFIED CHINESE_CHINA.ZHS16GBK
```

###二、修改plsql中的oci.dll地址
```bash
D:\app\instantclient_11_2\oci.dll
```

###三、Oracle局域网访问
1、修改`listener.ora`中的`HOST`为计算机全名
```bash
SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (SID_NAME = CLRExtProc)
      (ORACLE_HOME = C:\app\Administrator\product\11.2.0\dbhome_1)
      (PROGRAM = extproc)
      (ENVS = "EXTPROC_DLLS=ONLY:C:\app\Administrator\product\11.2.0\dbhome_1\bin\oraclr11.dll")
    )
  )

LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
      (ADDRESS = (PROTOCOL = TCP)(HOST = WIN-HE098SDJBMO)(PORT = 1521))
    )
  )

ADR_BASE_LISTENER = C:\app\Administrator
```

2、修改`tnsnames.ora`中的`HOST`为计算机全名
```bash
LISTENER_ORCL =
  (ADDRESS = (PROTOCOL = TCP)(HOST = WIN-HE098SDJBMO)(PORT = 1521))

ORACLR_CONNECTION_DATA =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
    (CONNECT_DATA =
      (SID = CLRExtProc)
      (PRESENTATION = RO)
    )
  )

ORCL =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = WIN-HE098SDJBMO)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )
```