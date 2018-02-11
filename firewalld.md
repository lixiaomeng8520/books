## 参考

1. [How to Configure ‘FirewallD’ in RHEL/CentOS 7 and Fedora 21](https://www.tecmint.com/configure-firewalld-in-centos-7/)
2. [Useful ‘FirewallD’ Rules to Configure and Manage Firewall in Linux](https://www.tecmint.com/firewalld-rules-for-centos-7/)

## 说明

1. firewalld代替iptables.
2. 两者不要同时存在.
3. iptables使用INPUT, OUTPUT, FORWARD链; 但是firewalld使用Zones(区域)的概念.
4. firewalld优势之一, 它预定义了一些服务.

## 安装

```bash
yum install firewalld
```

## zone(区域)

### 说明

1. 我们需要将interface赋予某个zone.
2. zone定义了interface是允许还是禁止连接.
3. zone可以包含服务或端口.
4. 服务是端口和选项的集合.

### zone分类

|zone名称|描述|
|-|-|
|Drop Zone|只出不进; 相当于iptables -j drop|
|Block Zone|禁止进入, 通过icmp-host-prohibited拒绝, 只允许本机内建立的连接.|
|Public Zone|允许特定的端口|
|External Zone|TODO: nat|
|DMZ Zone||
|Work Zone||
|Home Zone||
|Internal Zone||
|Trusted Zone||

## 命令

```bash
firewall-cmd --get-zones # 获取所有zone名称
firewall-cmd --get-default-zone # 获取默认zone
firewall-cmd --list-all-zones # 列出所有zone, 以及zone里面的配置信息
* firewall-cmd --zone=public --list-all
firewall-cmd --set-default-zone=public # 设置默认zone
firewall-cmd --get-icmptypes # 获取支持的icmp类型
* firewall-cmd --get-services # 获取所有服务
ls /usr/lib/firewalld/services # 列出系统所有默认自带服务
* firewall-cmd --reload # 重载配置
firewall-cmd --state # 状态
firewall-cmd --get-active-zones # 获取激活的zone
firewall-cmd --get-service # 获取默认网卡区域的服务
firewall-cmd --add-service=rtmp # 默认区域添加临时服务
firewall-cmd --zone=public --add-service=rtmp # 指定区域添加临时服务
* firewall-cmd --zone=public --add-service=rtmp --permanent # 指定区域添加永久服务
firewall-cmd --permanent --add-source=192.168.0.0/24 # 添加一个网段来源IP
firewall-cmd --permanent --add-port=1935/tcp # 添加一个tcp端口
```

## 添加自定义服务

### 1. 拷贝一份配置文件

```bash
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/rtmp.xml
```

### 2. 修改标题, 描述, 协议, 端口

```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>rtmp</short>
  <description>rtmp</description>
  <port protocol="tcp" port="1935"/>
</service>
```
