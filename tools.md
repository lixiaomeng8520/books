## 常用工具软件包

1. net-tools (base)
2. git2u (ius)
3. subversion (centos/updates)
4. lrzsz (与当前windows进行文件上传下载)

## nmcli 命令

|描述|命令|
|----|----|
|查看所有设备|nmcli d|
|查看所有连接|nmcli c|
|查看static这个连接|nmcli c show static|
|添加dhcp连接|nmcli c add con-name dhcp type ethernet ifname enp0s3|
|添加static连接|nmcli c add con-name static type ethernet ifname enp0s3 ip4 192.168.56.20/24 gw4 192.168.56.1|
|修改连接属性，参考show属性|nmcli c mod static ipv4.addresses 192.168.56.20/24|
|修改连接为静态连接|nmcli c mod static ipv4.method manual|
|启动static连接|nmcli c up static|
|关闭static连接|nmcli c down static|

## netstat 命令

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

## lrzsz

rz, sz便是Linux/Unix同Windows进行ZModem文件传输的命令行工具

1. rz (从windows receive接收文件)
2. sz a.txt (send发送文件a.txt到windows)

## curl

|描述|命令|
|-|-|
|下载|curl -L url -o target|
|POST|curl -X POST -H Content-Type:text/html url -d xxx|

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
