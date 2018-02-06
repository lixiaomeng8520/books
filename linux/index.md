## 注意

1. setenforce 0
2. 用户U在查看有读权限的文件F时, U必须对F的目录有可执行权限, 否则提示权限不够.

***

## 常用工具软件包

1. net-tools
2. git2u (ius)
3. php71u-fpm (ius)
4. nginx (epel)
5. subversion (centos/updates)

***

## 文件类型
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

## [systemd](systemd.md)

***

## centos7 uefi无法引导问题
1. 重新启动安装盘，进入rescure模式
2. cd /mnt/sysimage/boot/efi/EFI
3. cp centos/grubx64.efi BOOT/

***

## 网络

virtualbox采用nat+hostonly模式，nat用来上网，hostonly用来构建内部网络。
centos按照顺序加载网卡配置文件，所以先配置nat后配置hostonly的话，hostonly不要写网关，否则会覆盖掉nat。
TODO：先配置hostonly后配置nat。

nmcli命令

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

## 安装仓库

|描述|命令|
|----|----|
|epel|yum -y install epel-release|
|ius |yum -y install https://centos7.iuscommunity.org/ius-release.rpm|

***

## 软件包

|描述|命令|
|----|----|
|查看一个包的依赖|yum deplist php71u-cli|
|查询php71u是否安装|rpm -q php71u-fpm|
|查看php71u包信息|rpm -qi php71u-fpm|
|列出php71u包含的文件|rpm -ql php71u-fpm|
|查看filename属于哪个rpm包|rpm -qf filename|

***

## samba

1. yum install -y samba
2. smbpasswd -a lxm (添加一个linux用户,并输入密码)
3. systemctl start smb
4. 直接可以访问home目录,windows: \\\\192.168.56.20\lxm

***

## nginx

1. fastcgi.conf比fastcgi_params多了一行SCRIPT_FILENAME, 应该使用前者。[参考](https://blog.martinfjordvald.com/2013/04/nginx-config-history-fastcgi_params-versus-fastcgi-conf/)

***

## php

### 通过ius安装的php71u

1. yum install -y php71u-fpm (依赖: php71u-common)
2. --with-config-file-path=/etc/php.ini 和 --with-config-file-scan-dir php=/etc/php.d 配置文件指定在编译时的参数里
3. php71u-common 包含文件
    1. /etc/php.ini (主配置文件)
    2. /etc/php.d/* (各模块配置文件)
    3. /usr/lib64/php/modules/*.so (模块so文件)
4. php71u-mysqlnd 里包含了mysqlnd, mysqli, pdo_mysql