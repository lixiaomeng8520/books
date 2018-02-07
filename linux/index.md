### [install](install.md)

### [package](package.md)

### [systemd](systemd.md)

### [tools](tools.md)

### [samba](samba.md)

### [nginx](nginx.md)

### [php](php.md)

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

## centos7 uefi无法引导问题
1. 重新启动安装盘，进入rescure模式
2. cd /mnt/sysimage/boot/efi/EFI
3. cp centos/grubx64.efi BOOT/