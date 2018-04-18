## 安装仓库

|描述|命令|
|----|----|
|epel|yum -y install epel-release|
|ius |yum -y install https://centos7.iuscommunity.org/ius-release.rpm|

## 软件包

|描述|命令|
|----|----|
|查看一个包的依赖|yum deplist php71u-cli|
|查询php71u是否安装|rpm -q php71u-fpm|
|查看php71u包信息|rpm -qi php71u-fpm|
|列出php71u包含的文件|rpm -ql php71u-fpm|
|查看filename属于哪个rpm包|rpm -qf filename|

## 阿里云源

1. curl -L -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
2. curl -L -o /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo