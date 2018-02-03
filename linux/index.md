### 文件类型
1. - 普通文件
2. d 目录 
3. c 字符设备文件
4. b 块设备文件
5. s 套接口文件 如我们开启MySQL服务后，在/var/lib/mysql/下生成的mysql.sock文件，关闭MySQL服务后，这个文件就消失了
6. p 管道
7. l 符号链接文件

***

### 守护进程
systemctl (start|restart|stop|status) nginx

***

### centos7 uefi无法引导问题
1. 重新启动安装盘，进入rescure模式
2. cd /mnt/sysimage/boot/efi/EFI
3. cp centos/grubx64.efi BOOT/

***

### 网络(nat + hostonly)

| 命令  |  描述 |
|------|------|
|nmcli d|查看设备|
|nmcli c|查看所有连接|
|nmcli c show static|查看static这个连接|
|nmcli c add con-name static type ethernet ifname enp0s3 ip4 192.168.56.20/24 gw4 192.168.56.1|添加一个连接,加上ip4则bootproto改为静态|
|nmcli c mod static ipv4.addresses 192.168.56.20/24|改连接，通过show列出的属性进行设置|
|nmcli c static up|启动static连接|

natstat命令
1. netstat -a ()



### 安装仓库
1. yum -y install epel-release (epel)
2. yum -y install https://centos7.iuscommunity.org/ius-release.rpm (ius)

### 软件包
1. rpm -q php71u (查询php71u是否安装)
2. rpm -qi php71u (查看php71u包信息)
3. rpm -ql php71u (列出php71u包含的文件)
4. rpm -qf filename (查看filename属于哪个rpm包)
