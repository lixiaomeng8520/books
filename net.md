## 参考

1. [网络传输中的三张表，MAC地址表、ARP缓存表以及路由表详解](https://www.jianshu.com/p/63fd0faa47da)
2. [linux 多网卡路由问题](https://blog.csdn.net/yuanbinquan/article/details/51468886)

## 交换机

本质需要每次连接都需要修改源MAC和目标MAC, 通过MAC来进行标识. 如果不知道对方MAC, 则发送广播.

1. 两层: MAC
2. 三层: IP

## 数据包请求流程(来源: 百度知道)

1. A在给B发数据时，A和B的IP地址均与A的子网掩码进行与运算，相同则为同一网段，因为B的子网掩码是不知道的。
2. 如果发现与B是同一网段那么广播发送即可。
3. 如果发现与B不在同一网段，那么A会查找网关（A在电脑上配置的）。
4. 发送ARP请求获得网关MAC地址。
5. 获得MAC后，数据包中原IP为A的IP，目的IP为B的IP，原MAC地址为A的MAC地址，目的MAC地址为A网关的地址，这样A就将包发给了网关。
6. A网关收到数据包后查看，根据目的IP（B的IP地址）查找路由表，找到通往目的网段的路由及下一跳，得到下一跳的MAC地址（ARP获得的），然后将数据包中原IP、目的IP保持不变，原MAC地址换成A网关的MAC地址，目的地址换成下一跳的MAC地址，转发到下一跳的设备（路由器，三层交换机等）。
7. 如果下一跳就是B的网关（不是B的网关，就重复上面的动作），网关收到后查看，发现目的IP在自己的内部（ARP表），将数据包中原IP、目的IP保持不变，原MAC地址换成B网关的MAC地址，目的地址换成B的MAC地址，将数据包发给B，B得到数据包后，完成A与B的通信。

## 路由转发

所谓转发即当主机拥有多于一块的网卡时，其中一块收到数据包，根据数据包的目的ip地址将数据包发往本机另一块网卡，该网卡根据路由表继续发送数据包。这通常是路由器所要实现的功能。

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward # 开启路由转发
```



## virtualbox

1. hostonly相当于各主机通过双绞线相连, 一个interface(界面)相当于一个交换机, 可以构成独立的物理网络.
2. 一个主机需要选择一个interface进行连接.
3. 连接两个interface的主机可以做路由, 连接两个物理网络.
4. 路由主机需要开启转发, echo 1 > /proc/sys/net/ipv4/ip_forward.
5. virtualbox可以采用nat + hostonly模式, nat用来上网, hostonly用来构建内部网络.
6. centos按照顺序加载网卡配置文件, 所以先配置nat后配置hostonly的话，hostonly不要写网关, 否则会覆盖掉nat.

## DHCP

### 流程

1. 客户端广播
2. 服务器响应

### 注意

1. 不能分配和当前网段不一致的网段.
2. 如果要分配其他网段, 可以添加一块网卡, 或者在当前网卡上添加一个其他网段IP.


## route 命令

### 显示本机路由表

```
route -n

Destination     Gateway         Genmask Flags Metric Ref    Use Iface
192.168.0.0     *               255.255.255.0   U     0      0        0 eth0
169.254.0.0     *               255.255.0.0     U     0      0        0 eth0
default         192.168.0.1     0.0.0.0         UG    0      0        0 eth0
```

|输出项|说明|
|-|-|
|Destination|目标网段或者主机|
|Gateway|网关地址，”*” 表示目标是本主机所属的网络，不需要路由|
|Genmask|网络掩码|
|Flags| U — 路由是活动的 |
|     | H — 目标是一个主机|
|     | G — 路由指向网关 |
|     | R — 恢复动态路由产生的表项 |
|     | D — 由路由的后台程序动态地安装 |
|     | M — 由路由的后台程序修改 |
|     | ! — 拒绝路由|
|Metric|路由距离，到达指定网络所需的中转数（linux 内核中没有使用）|
|Ref|路由项引用次数（linux 内核中没有使用）|
|Use|此路由项被路由软件查找的次数|
|Iface|该路由表项对应的输出接口|

### 添加删除路由

```
route  [add|del] [-net|-host] target [netmask Nm] [gw Gw] [[dev] If]

# 添加到主机的路由
route add -host 192.168.1.2 dev eth0 
route add -host 10.20.30.148 gw 10.20.30.40     #添加到10.20.30.148的网管

# 添加到网络的路由
route add -net 10.20.30.40 netmask 255.255.255.248 eth0 #添加10.20.30.40的网络
route add -net 10.20.30.48 netmask 255.255.255.248 gw 10.20.30.41 #添加10.20.30.48的网络
route add -net 192.168.1.0/24 eth1

# 添加默认路由
route add default gw 192.168.1.1

# 删除路由
route del -host 192.168.1.2 dev eth0:0
route del -host 10.20.30.148 gw 10.20.30.40
route del -net 10.20.30.40 netmask 255.255.255.248 eth0
route del -net 10.20.30.48 netmask 255.255.255.248 gw 10.20.30.41
route del -net 192.168.1.0/24 eth1
route del default gw 192.168.1.1
```


## tcpdump抓包

### 参考

### 命令

|描述|命令|
|-|-|
|不解析域名|tcpdump -n|
|两个主机之间的数据|tcpdump -n -i eth0 host 192.168.31.147 and 114.114.114.114|
|目的IP数据|tcpdump -n -i eth0 dst 192.168.31.147 or 192.168.31.157|
|从本机出去的数据包|tcpdump -n -i eth0 src 192.168.31.147 or 192.168.31.157|
||tcpdump -n -i eth0 src 192.168.31.147 or 192.168.31.157 and port ! 22 and tcp|

