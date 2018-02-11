## 安装

yum方式有很多仓库，比如ius, remi. 软件包都已经规划好目录, 不要用混淆.

这里采用ius仓库:

1. yum install -y php71u-fpm (cgi, 包含php-fpm. 默认只安装这个就行)
2. yum install -y php71u-cli (cli, 包含php, phpize)
3. yum install -y php71u-mysqlnd (包含mysqlnd, mysqli, pdo_mysql)

基本都依赖php71u-common, 主要包括下列:

1. /etc/php.ini (主配置文件)
2. /etc/php.d/* (各模块配置文件)
3. /usr/lib64/php/modules/*.so (模块so文件)

## php-fpm

### 启动

systemctl start php-fpm
systemctl enable php-fpm
systemctl status php-fpm

php-fpm启动, 主进程会寻找php配置文件, 他们默认指定在编译时的参数里

```sh
--with-config-file-path=/etc/php.ini
--with-config-file-scan-dir=/etc/php.d/
```

也可以在启动时指定

```sh
php-fpm -c /etc/php.ini
```

> 没有这些配置文件也可以启动
