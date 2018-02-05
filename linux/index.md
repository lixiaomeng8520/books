### 注意
setenforce 0

***

### 文件类型
| 字符 | 描述 |
|----|----|
| \- |普通文件|
| d  |目录|
| c  |字符设备文件|
| b  |块设备文件|
| s  |套接口文件 如我们开启MySQL服务后，在/var/lib/mysql/下生成的mysql.sock文件，关闭MySQL服务后，这个文件就消失了|
| p  |管道|
| l  |符号链接文件|

***

### 守护进程(TODO)
systemctl (start|restart|stop|status) nginx

***

### centos7 uefi无法引导问题
1. 重新启动安装盘，进入rescure模式
2. cd /mnt/sysimage/boot/efi/EFI
3. cp centos/grubx64.efi BOOT/

***

### 网络(nat + hostonly)
nmcli, 一个设备可以对应多个连接

|描述|命令|
|----|----|
|查看所有设备|nmcli d|
|查看所有连接|nmcli c|
|查看static这个连接|nmcli c show static|
|添加dhcp连接|nmcli c add con-name dhcp type ethernet ifname enp0s3|
|添加static连接|nmcli c add con-name static type ethernet ifname enp0s3 ip4 192.168.56.20/24 gw4 192.168.56.1|
|修改连接属性，参考show属性|nmcli c mod static ipv4.addresses 192.168.56.20/24|
|启动static连接|nmcli c up static|
|关闭static连接|nmcli c down static|

netstat 是一款命令行工具，可用于列出系统上所有的网络套接字连接情况，包括 tcp, udp 以及 unix 套接字，另外它还能列出处于监听状态（即等待接入请求）的套接字。

|描述|命令|
|----|----|
|列出所有|netstat -a|
|列出tcp|netstat -at|
|列出udp|netstat -au|
|禁用域名解析|netstat -ant|
|*监听中的连接|netstat -ntl|
|获取进程名|netstat -ntlp|
|获取进程用户|netstat -ntlpe|
|获取内核路由|netstat -nr|

***

### 安装仓库
|描述|命令|
|----|----|
|epel|yum -y install epel-release|
|ius |yum -y install https://centos7.iuscommunity.org/ius-release.rpm|

***

### 软件包
|描述|命令|
|----|----|
|查询php71u是否安装|rpm -q php71u|
|查看php71u包信息|rpm -qi php71u|
|列出php71u包含的文件|rpm -ql php71u|
|查看filename属于哪个rpm包|rpm -qf filename|

***

### samba
1. yum install -y samba
2. smbpasswd -a lxm (添加一个linux用户,并输入密码)
3. systemctl start smb
4. 直接可以访问home目录,windows: \\\\192.168.56.20\lxm