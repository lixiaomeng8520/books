## 仓库

1. base
2. epel
3. ius
4. remi


## 阿里云源

1. curl -L -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
2. curl -L -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo


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