### 命令
|管理系统|systemctl|
|启动耗时|systemd-analyze|
|主机的信息|hostnamectl|
|本地化设置|localectl|
|时间设置|timedatectl|
|登录的用户|loginctl|

### 电源
|描述|命令|
|----|----|
|关机|systemctl poweroff|
|重启|systemctl reboot|

### Systemd 可以管理所有系统资源。不同的资源统称为 Unit

|系统服务|Service|
|多个Unit构成的一个组|Target|
|硬件设备|Device|
|文件系统的挂载点|Mount|
|自动挂载点|Automount|
|文件或路径|Path|
|不是由Systemd启动的外部进程|Scope|
|进程组|Slice|
|Systemd快照,可以切回某个快照|Snapshot|
|进程间通信的socket|Socket|
|swap文件|Swap|
|定时器|Timer|

### unit管理

|正在运行unit|systemctl list-units|
|所有unit|systemctl list-units --all|
|类型为service,正在运行|systemctl list-units --type=service|
|unit状态|systemctl status nginx|
|启动service|systemctl start nginx|
|停止service|systemctl stop nginx|
|重启service|systemctl restart nginx|
|重载配置文件|systemctl reload nginx|
|杀死服务所有子进程|systemctl kill nginx|
|显示一个unit参数|systemctl show nginx|
|开机启动|systemctl enable nginx|
|开机不启动|systemctl disable nginx|

### 配置文件
/etc/systemd/system/ -> /usr/lib/systemd/system/

|列出unit files|systemctl list-unit-files --type=service|
|列出单个unit files|systemctl list-unit-files smb.service|