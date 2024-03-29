# Linux下部署SMB Server

## 安装SMB Server  
**Ubuntu安装：**`apt install samba samba-common`

---

## 配置文件  
**所在目录：**`/etc/samba/smb.conf`  

*创建共享文件夹示例>>>添加与配置文件末尾*
```
[share]                                    # 自定义共享名
comment=this is Linux share directory    # 描述符，是给系统管理员看的
path=/home/ubuntu/share                    # 共享的路径
public=yes                                # 是否公开，也就是是否能在网上邻居看到该共享
writable=yes                            # 是否可写
```

*更多参数详解*
```
[共享文件夹]                                      # 自定义共享名
      comment = Home Directories                 # 描述符，是给系统管理员看的
      path = /home/share                         # 共享的路径
      public = yes                               # 是否公开，也就是是否能在网上邻居看到该共享
      browseable = yes                           # 共享的目录是否让所有人可见
      writable = yes                             # 是否可写
      guest ok = no                              # 是否拒绝匿名访问，仅当安全级别为 share 时才生效
      workgroup = WORKGROUP                      # 工作组，要设置成跟 Windows 的工作组一致
      server string = Samba Server Version %v    # 其他 Linux 主机查看共享时的提示符
      netbios name = MYSERVER                    # 用于在 Windows 网上邻居上显示的主机名
      hosts allow = 127. 192.168.12. 192.168.13. EXCEPT 192.168.13.13       # 指定允许访问 samba 服务器的主机   
      security = share                           # 定义安全级别
      log file = /var/log/samba/log.%m           # 定义日志文件，每个访问的主机会产生独立的日志文件，%m 是客户端主机名
      max log size = 50                          # 定义单个日志的最大容量（KB）
      passdb backend = tdbsam                    # Samba 用户的存储方式，smbpasswd 表示明文存储，tdbsam 表示密文存储
      deadtime = 10                              # 客户端在10分钟内没有打开任何 Samba 资源，服务器将自动关闭会话，在大量的并发访问环境中，这样的设置可以提高服务器性能
      display charset = UTF8                     # 设置显示的字符集
      max connections = 0                        # 设置最大连接数，0表示无限制，如果超过最大连接数则拒绝连接
      guest account = nobody                     # 设置匿名账户为nobody
      load printers = yes                        # 是否在启动 Samba 时就共享打印机   
      cups options = raw                         # 设置打印机使用的方式
      valid users = user1 user2    user3         # 指定哪些用户可以访问，如果不指定则所有用户都可访问
      invalid users = user1 user2                # 指定哪些用户不可以访问
      create mask = 0775                         # 客户端上传文件的默认权限
      directory mask = 0775                      # 客户端创建目录的默认权限
      write list = user1 user2 user3             # 设置可对文件进行写操作的用户
      admin users = user1                        # 设置共享目录的管理员，具有完全权限
```

---

## 常用指令

创建帐号及密码管理：`smbpasswd`
*samba用户需要以系统用户为基础，才能创建为samba用户*
**常用参数**
```
smbpasswd -a                                     # 添加用户
smbpasswd -x                                     # 删除用户
smbpasswd -d                                     # 冻结用户
smbpasswd -n                                     # 密码重置
```

查询已经存在的samba用户：`pdbedit -L`  

---

## 常見問題

1. Samba不允许同一台电脑用两个不同的账户连接--*解决方式WindowsCMD输入*`net use * /del /y`  