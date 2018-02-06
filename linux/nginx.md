## nginx配置php

```nginx
server {
    listen 80;
    server_name test.lxm.cn;
    index index.html index.php;

    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        include fastcgi.conf;
    }
}
```

## nginx location规则

### 语法:
1. location [ = | ~ | ~* | ^~ ] uri { ... }
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

1. 首先遵循大的优先级
2. 同级别内
    1. 正则匹配成功之后停止匹配，非正则匹配成功还会接着匹配
    2. 在所有匹配成功的url中，选取匹配度最大的url字符地址


## 杂项

1. fastcgi.conf比fastcgi_params多了一行SCRIPT_FILENAME, 应该使用前者。[参考](https://blog.martinfjordvald.com/2013/04/nginx-config-history-fastcgi_params-versus-fastcgi-conf/)
