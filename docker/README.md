## Docker

### Ubuntu

系统要求：

- Disco 19.04
- Cosmic 18.10
- Bionic 18.04 (LTS)
- Xenial 16.04 (LTS)

```shell
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```  

如果系统版本 < 14.04 :

```shell
# 首先要更新内核
sudo apt-get update
sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
sudo reboot
sudo apt-get install apt-transport-https
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
sudo apt-get update
sudo apt-get install lxc-docker apparmor
```  

### CentOS

在 CentOS7 版本中:

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker
sudo systemctl start docker
```

如果是 CentOS6 :

```shell
sudo yum install http://mirrors.yun-idc.com/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo yum install docker-io
# 添加用户到docker组  
sudo gpasswd -a ${USER} docker
# 启动服务  
sudo service docker start
```

### docker 基本使用

- 镜像加速  
在国内拉取官方镜像，速度简直不堪入目。所以要使用加速器，这里我推荐使用 aliyun 或者 daocloud 加速器，在官网注册账号，然后根据提示操作即可。  

- 搜索并拉取镜像
  
```shell
docker search <image-name>
docker pull <image-name>
```  

- 查看镜像  

```$ docker images```

- 运行容器  
启动一个新容器  
```$ docker run -t -i <image-name>```  
其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开  
启动已终止容器  
```$ docker start <ID>```  
如果需要容器在后台保持运行，添加参数 -d  

- 查看 all 容器  
```$ docker ps -a```  

- 终止容器  
```$ docker stop <ID>```  

- 进入容器  
使用docker attach <ID>  
使用nsenter  

- 提交更改  
```$ docker commit -m "说明" -a "维护者信息" 容器ID 目标镜像仓库名:tag```  

- push镜像  
```$ docker push 目标镜像仓库名:tag```  

- 移除  
移除镜像  
```docker rmi image-name```  
移除容器  
```docker rm ID```  
移除所有容器  
```for i in `docker ps -a|awk '{print $1}'|grep -v CONTAINER`;do docker rm $i;done```  

- 数据卷  
使用-v 创建数据卷并挂载到容器中  
```docker run -t -i -v /data:/data --name volumes centos /bin/bash```  
如果需要在多容器共享数据，可以使用数据卷容器:

```shell
docker run -v /data:/data --name data centos echo This is DATA VOLUMES
docker run -t -i --volumes-from data centos /bin/bash
```

数据卷备份:

```docker run -v $(PWD):/tmp --volumes-from data centos tar zcf /tmp/data.tgz /data```  

数据卷导入:

```docker run -v $(PWD):/tmp --volumes-frpm data centos tar zxf /tmp/data.tgz -C /data```  

### Docker-compose

安装 compose 工具:

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
