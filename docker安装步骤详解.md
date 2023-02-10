**前言：本章学习如何安装docker，以及优化**

# 一：[docker安装](https://so.csdn.net/so/search?q=docker安装&spm=1001.2101.3001.7020)步骤详解

docker初期版本是1.13（同一版本，开源）——》分类型 1.15 - 1.17 过程中分成两种。

①开源社区 docker-ce

②企业版 docker-ee

```bash
目前 Docker 只能支持 64 位系统。
 
1.#关闭防火墙
systemctl stop firewalld.service
systemctl disable firewalld.service
setenforce 0
 
2.#安装依赖包
yum install -y yum-utils device-mapper-persistent-data lvm2 
-----------------------------------------------------------------------------
#yum-utils：提供了 yum-config-manager 工具。
#device mapper： 是Linux内核中支持逻辑卷管理的通用设备映射机制，它为实现用于存储资源管理的块设备驱动提供了一个高度模块化的内核架构。
#device mapper存储驱动程序需要 device-mapper-persistent-data 和 lvm2。
-----------------------------------------------------------------------------
 
3.#设置阿里云镜像源
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 
 
4.#安装 Docker-CE 社区版并设置为开机自动启动
yum install -y docker-ce
 
systemctl start docker.service
systemctl enable docker.service 
 
5.#查看 docker 版本信息
docker version
```

## 1.1关闭防火墙

```bash
systemctl stop firewalld.service

#关闭增强机制，取消开机自启
systemctl disable firewalld.service
setenforce 0
```

## 1.2安装依赖包

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2 
-----------------------------------------------------------------------------
#yum-utils：提供了 yum-config-manager 工具。
#device mapper： 是Linux内核中支持逻辑卷管理的通用设备映射机制，它为实现用于存储资源管理的块设备驱动提供了一个高度模块化的内核架构。
#device mapper存储驱动程序需要 device-mapper-persistent-data 和 lvm2。
-----------------------------------------------------------------------------
```

## 1.3设置阿里云镜像源

```bash
cd /etc/yum.repos.d/
yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## 1.4安装 Docker-CE并设置为开机自动启动

```bash
yum install -y docker-ce
 
systemctl start docker.service
systemctl enable docker.service
```

##  1.5查看 docker 版本信息

```bash
docker version
```

# 二：设置镜像加速

**申请加速器地址**

这里我们使用阿里云的镜像，加速镜像下载速度

注册阿里云账号并登录 

https://blog.csdn.net/qq_44239779/article/details/127292446

# 三：网络优化

```bash
vim /etc/sysctl.conf
net.ipv4.ip_forward=1  #开启路由转发
 
sysctl -p   #刷新
systemctl restart network
systemctl restart docker
```

# 四：docker-server端配置文件建议配置

```json
#在daemon. json文件中配置
{
  "graph":"/data/docker",		#数据目录，数据存储位置
  "storage-driver":"overlay2",	#存储引擎，docker1.18以上版本默认使用overlay2存储引擎。早期的适合存储引擎使用的aufs
  "insecure-registries":["registry.access.redhat.com","quary.io"],	#私有仓库
  #"registry-mirrors": [ "https:/lq" ]  #镜像加速，在这个下面添加
  "bip":"172.137.20.1/24",		#docker网络
  "exec-opts":["native.cgroupdriver=systemd"],	#启动时的额外参数，（是一种挂载驱动，k8s使用）
  "live-restore":true	#当docker容器引擎挂掉的时候，使用docker跑起来的容器还能继续运行
}
 
以上是建议的配置项
docker的网络建议和宿主机的IP"对照"
比如宿主机10.2.5.6 容器的地址就可以修改为172.5.6.1，这样方便在故障发生时，更容易定位故障节点的位置
 
systemctl daemon-reload  #重载守护进程
systemctl restart docker #重载引擎
```

```json
{
  "graph":"/data/docker",
  "storage-driver":"overlay2",
  "insecure-registries":["registry.access.redhat.com","quary.io"],
 
  "bip":"172.137.20.1/24",
  "exec-opts":["native.cgroupdriver=systemd"],
  "live-restore":true
}
 
systemctl daemon-reload
systemctl restart docker
```

# 五：总结

本章主要是学习如何安装docker以及进行一些简单的优化