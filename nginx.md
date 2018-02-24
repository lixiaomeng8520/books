## 参考文章

1. [简明 Nginx Location Url 配置笔记](https://www.jianshu.com/p/e154c2ef002f)
2. [FASTCGI_PARAMS VERSUS FASTCGI.CONF – NGINX CONFIG HISTORY](https://blog.martinfjordvald.com/2013/04/nginx-config-history-fastcgi_params-versus-fastcgi-conf/)
3. [How to Install Nginx on CentOS 7](https://www.tecmint.com/install-nginx-on-centos-7/)


## nginx配置php

这里nginx如果报primary...什么的, 应该是对该文件没有访问权限. 要检查这个文件的父目录们, 需要有x权限.

```nginx
server {
    listen 80;
    server_name test.lxm.cn;
    index index.html index.php;
    root /home/lxm/workspace/php/test;

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        include fastcgi.conf;
    }
}
```


## nginx location规则

### 语法:
1. location [ = | ~ | ~\* | ^\~ ] uri { ... }
2. location @name { ... }

### 匹配模式

|优先级|模式|语法|区分大小写|使用正则|描述|
|-|-|-|-|-|-|
|1|精确匹配|location = /demo {} |是|否||
|2|前缀匹配|location ^~ /demo {}|是|否|常用于静态文件夹|
|3|正则匹配|location ~ /demo {} |是|是||
|3|正则匹配|location ~* /demo {}|否|是||
|4|正常匹配|location /demo {}   |否|是||
|5|全匹配|location / {}|是|否||

1. 首先遵循大的优先级.
2. 同级别内.
    1. 正则匹配成功之后停止匹配，非正则匹配成功还会接着匹配.
    2. 在所有匹配成功的url中，选取匹配度最大的url字符地址.

## 变量

|变量名|描述|
|-|-|
|uri|域名后/到第一个?之前|
|is_args|第一个?|
|args|第一个?之后的内容|
|request_uri|域名后/到最后|
|fastcgi_script_name|目前看是同uri|


## 杂项

1. fastcgi.conf比fastcgi_params多了一行SCRIPT_FILENAME, 应该使用前者.
2. fastcgi_split_path_info会根据正则表达式重新赋值两个变量`fastcgi_script_name`和`fastcgi_path_info`, 如下示例: 
```nginx
location ~ ^(.+\.php)(.*)$ {
    fastcgi_split_path_info       ^(.+\.php)(.*)$;
    fastcgi_param SCRIPT_FILENAME /path/to/php$fastcgi_script_name;
    fastcgi_param PATH_INFO       $fastcgi_path_info;
}
```
