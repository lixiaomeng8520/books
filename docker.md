## 参考

1. [Install Docker and Learn Basic Container Manipulation in CentOS and RHEL 7/6 – Part 1](https://www.tecmint.com/install-docker-and-learn-containers-in-centos-rhel-7-6/)
2. [How to Install, Run and Delete Applications inside Docker Containers – Part 2](https://www.tecmint.com/install-run-and-delete-applications-inside-docker-containers/)
3. [使用 Docker 部署 Ceph 集群](https://www.jianshu.com/p/f08ed7287416)
4. [ceph/daemon]https://hub.docker.com/r/ceph/daemon/()

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

### 网络

|查看|docker network ls|
|自定义|docker network create --subnet=172.18.0.0/16 mybridge|

### docker run 相关参数

|参数|描述|
|-|-|
|--restart|always, 容器退出时重启策略|


## registry

### 资料

1. [Docker Registry](https://docs.docker.com/registry/)
2. [Automated Docker Deployment using Jenkins](http://www.tothenew.com/blog/automated-docker-deployment-using-jenkins/)

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


## swarm

1. [Swarm mode overview](https://docs.docker.com/engine/swarm/)


## note

1. 每运行一次image, 都会生成一个容器, 就算命令是一样的.
2. image命名: ip/namespace/name.
3. 默认网络不支持固定IP, 需要创建自定义网络.
4. --privileged=true 使用该参数，container内的root拥有真正的root权限。
否则，container内的root只是外部的一个普通用户权限。


## 试验

```
docker pull jenkins
docker pull gitlab/gitlab-ce



docker run -d -p 5000:5000 -p 5001:5001 --restart always --name registry -v /root/registry/config.yml:/etc/docker/registry/config.yml --network mybridge --ip 172.18.0.2 registry
http://admin:39e2dd0f48a93b0f233070b50a305b9e@172.17.0.3:8080/dockerregistry-webhook/notify

docker run -d --restart always -p 8080:8080 -p 50000:50000 --group-add=`stat -c %g /var/run/docker.sock` --network mybridge --ip 172.18.0.3 --name jenkins -v /root/jenkins/:/var/jenkins_home/ -v /root/download/docker/docker:/usr/local/bin/docker -v /var/run/docker.sock:/var/run/docker.sock jenkins

curl -L https://get.docker.com/builds/Linux/x86_64/docker-1.13.1.tgz -o docker.tgz
tar -zxvf docker.tgz


bouncycastle

docker run -d --restart always -p 8081:80 --name gitlab -v /root/gitlab/etc/gitlab:/etc/gitlab -v /root/gitlab/var/opt/gitlab:/var/opt/gitlab -v /root/gitlab/var/log/gitlab:/var/log/gitlab --network mybridge --ip 172.18.0.4 gitlab/gitlab-ce 
172.18.0.4
root babyhannah
http://172.17.0.4/root/mydocker.git


docker run -d --name mysql -v /root/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=111 --network mybridge --ip 172.18.0.5 mysql

```

```
node {
   stage('get source') {
       git branch: 'master', credentialsId: 'fe220e94-9b54-44be-98d7-c3a532fe813a', url: 'git@172.18.0.4:root/dahecj_web.git'
   }
   stage('npm build') {
        nodejs('node') {
            sh 'cnpm install'
            sh 'npm run build'
        }
   }
    stage('docker build') {
        git branch: 'master', credentialsId: 'fe220e94-9b54-44be-98d7-c3a532fe813a', url: 'git@172.18.0.4:root/mydocker.git'
        sh 'cp -r dist dahecj_web/'
        sh 'docker build -t 172.18.0.2:5000/lxm/dahecj_web --build-arg source=dist dahecj_web'
        sh 'docker push 172.18.0.2:5000/lxm/dahecj_web'
    }
}

node {
    stage('pull') {
        sh 'docker pull 172.18.0.2:5000/lxm/dahecj_web'
    }
    stage('stop old') {
        sh 'docker rm -f dahecj_web'
    }
    stage('start new') {
        sh 'docker run -d -p 7000:7000 --name dahecj_web 172.18.0.2:5000/lxm/dahecj_web'
    }
}
```

```
curl -X POST http://172.17.0.1:2375/build -d "t=172.17.0.2%3a5000%2flxm%2fhello&remote=http%3a%2f%2froot%3ababyhannah%40172.17.0.4%2froot%2fhello.git"
curl -X POST -H "X-Registry-Auth:ddd" http://172.17.0.1:2375/images/172.17.0.2%3a5000%2flxm%2fhello/push

curl -X POST http://172.17.0.1:2375/images/create -d "fromImage=172.17.0.2%3a5000%2flxm%2fhello"

curl -X DELETE http://172.17.0.1:2375/containers/hello?force=true

curl -X POST -H "Content-Type: application/json" http://172.17.0.1:2375/containers/create?name=hello -d '{
    "Image": "172.17.0.2:5000/lxm/hello",
    "HostConfig": {
        "PortBindings": {
            "8082/tcp": [
                {
                    "HostPort": "80"
                }   
            ]
        }
    }
}'

curl -X POST http://172.17.0.1:2375/containers/hello/start
```

```
scp -r ~/ceph/ root@192.168.56.22:~


docker run -d --net=host --name=mon \
    -v /root/ceph/etc/ceph:/etc/ceph \
    -v /root/ceph/var/lib/ceph:/var/lib/ceph \
    -e MON_IP=192.168.56.21 \
    -e CEPH_PUBLIC_NETWORK=192.168.56.0/24 \
    -e MON_NAME=mon1 \
    ceph/daemon mon

docker run -d --net=host --name=mon \
    -v /root/ceph/etc/ceph:/etc/ceph \
    -v /root/ceph/var/lib/ceph:/var/lib/ceph \
    -e MON_IP=192.168.56.22 \
    -e CEPH_PUBLIC_NETWORK=192.168.56.0/24 \
    -e MON_NAME=mon2 \
    ceph/daemon mon

docker run -d --net=host --name=mon \
    -v /root/ceph/etc/ceph:/etc/ceph \
    -v /root/ceph/var/lib/ceph:/var/lib/ceph \
    -e MON_IP=192.168.56.24 \
    -e CEPH_PUBLIC_NETWORK=192.168.56.0/24 \
    -e MON_NAME=mon3 \
    ceph/daemon mon


docker run -d --net=host --name=osd \
    --privileged=true --pid=host\
    -v /root/ceph/etc/ceph:/etc/ceph \
    -v /root/ceph/var/lib/ceph:/var/lib/ceph \
    -v /dev/:/dev/ \
    -e OSD_TYPE=disk \
    -e OSD_DEVICE=/dev/sdb \
    -e OSD_FORCE_ZAP=1 \
    ceph/daemon osd


docker run -d --net=host \
-v /var/lib/ceph/:/var/lib/ceph/ \
-v /etc/ceph:/etc/ceph \
-e CEPHFS_CREATE=1 \
ceph/daemon mds



docker exec mon ceph -s

```

```
cat << EOM > /etc/yum.repos.d/ceph.repo
[ceph-noarch]
name=Ceph noarch packages
baseurl=https://download.ceph.com/rpm-luminous/el7/noarch
enabled=1
gpgcheck=1
type=rpm-md
gpgkey=https://download.ceph.com/keys/release.asc
EOM
```

## ceph

集群

|描述|命令|
|-|-|
|集群状态|ceph -s; ceph status|
|磁盘信息|ceph df|

mon

|描述|命令|
|-|-|
|mon状态|ceph mon stat; ceph mon dump|
|mon的选举状态|ceph quorum_status|
|mon的映射信息|ceph mon dump|
|删除一个mon节点|ceph mon remove cs1|
|获得一个正在运行的mon map 保存在1.txt文件中|ceph mon getmap -o 1.txt|
|读取上面获得的map|monmaptool --print 1.txt |

osd

|描述|命令|
|-|-|
|osd列表|ceph osd tree|
|删除osd|ceph osd rm 0|