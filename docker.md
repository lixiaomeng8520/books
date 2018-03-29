## 参考

1. [Install Docker and Learn Basic Container Manipulation in CentOS and RHEL 7/6 – Part 1](https://www.tecmint.com/install-docker-and-learn-containers-in-centos-rhel-7-6/)
2. [How to Install, Run and Delete Applications inside Docker Containers – Part 2](https://www.tecmint.com/install-run-and-delete-applications-inside-docker-containers/)

## 安装

```bash
# centos7
yum install docker

systemctl start docker 
systemctl status docker
systemctl enable docker

# centos6
yum install epel-release
yum install docker-io

service docker start
service docker status
chkconfig docker on
```

## 国内镜像

```bash
vim /etc/docker/daemon.json

{
    "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

## 基础命令

### docker相关

|描述|命令|
|-|-|
|列出所有命令|docker|
|docker信息|docker info|
|docker版本|docker version|
|列出bridge的所有网络|docker network inspect bridge|

### image相关

|描述|命令|
|-|-|
|列出images|docker images|
|给image打tag|docker tag ubuntu localhost:5000/my-ubuntu|
|搜索docker|docker search ubuntu -> docker pull docker.io/library/ubuntu|
|下载docker|docker pull ubuntu|
|下载docker|docker pull myregistrydomain:port/foo/bar|
|删除image|docker rmi ubuntu|
|删除所有image|docker rmi $(docker images -q)|

### 容器相关

|描述|命令|
|-|-|
|列出最近容器|docker ps -l|
|列出所有容器|docker ps -a|
|创建容器|docker run ubuntu cat /etc/issue|
|创建容器, 给容器命名|docker run --name myname ubuntu cat /etc/issue|
|删除容器|docker rm c629b7d70666|
|删除所有容器|docker rm $(docker ps -a -q)|
|运行已创建容器|docker start c629b7d70666|
|停止正在运行容器|docker stop c629b7d70666|
|容器状态|docker stats myname|
|进入ubuntu shell|docker run -it ubuntu bash|
|退出ubuntu shell, 停止运行|exit|
|退出ubuntu shell, 但不停止运行|Ctrl+p Ctrl+q|
|重新连接正在运行容器|docker attach c629b7d70666|
|终止正在运行容器|docker kill c629b7d70666|
|容器提交为image|docker commit container-name image-name|

### docker run 相关参数

|参数|描述|
|-|-|
|--restart|always, 容器退出时重启策略|


## registry

### 资料

1. [Docker Registry](https://docs.docker.com/registry/)

### 启动命令

```bash
# 自动启动
docker run -d -p 5000:5000 --restart=always --name registry registry:2

# 暴露端口
docker run -d -p 5001:5000 --name registry-test registry:2

# 定制内部端口 REGISTRY_HTTP_ADDR
docker run -d -e REGISTRY_HTTP_ADDR=0.0.0.0:5001 -p 5001:5001 --name registry-test registry:2

# 本地存储
docker run -d -p 5000:5000 --restart=always --name registry -v /mnt/registry:/var/lib/registry registry:2

# 远程存储到aws等
```




## note

1. 每运行一次image, 都会生成一个容器, 就算命令是一样的.
2. image命名: ip/namespace/name


