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

## ssh

|描述|命令|
|-|-|
|生成密钥|ssh-keygen -t rsa|
|拷贝公钥|ssh-copy-id root@master; ssh-copy-id -i ~/.ssh/id_rsa.pub root@master|

## 软件包

|描述|命令|
|----|----|
|查看一个包的依赖|yum deplist php71u-cli|
|查询php71u是否安装|rpm -q php71u-fpm|
|查看php71u包信息|rpm -qi php71u-fpm|
|列出php71u包含的文件|rpm -ql php71u-fpm|
|查看filename属于哪个rpm包|rpm -qf filename|
