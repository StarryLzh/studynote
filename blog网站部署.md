# 部署



## 1.Linux安装环境



### 1.1安装docker

```shell
yum install -y yum-utils device-mapper-persistentdata lvm2   //安装必要工具
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo   //设置yum源
yum install -y docker-ce  //下载docker
Systemctl start docker  //启动docker
```

### 1.2安装mysql

用Xftp在user/local下新建三个文件夹

```shell
docker pull mysql //下载MySQL镜像

//创建映射mysql配置路径
mkdir -p /usr/blog/mysql/conf //mysql配置路径
mkdir -p /usr/blog/mysql/data //数据路径
mkdir -p /usr/blog/mysql/logs //日志路径

//新建一个mysql的配置文件
vim /usr/blog/mysql/conf/my.cnf


docker run -d -p 3307:3307 --privileged=true -v /usr/blog/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /usr/blog/mysql/logs:/logs -v /usr/blog/mysql/data:/var/lib/mysql --name mysql -e MYSQL_ROOT_PASSWORD=ZHUOHAN2022 -e TZ=Asia/Shanghai mysql

//我设置的密码  TZ为时区
docker run  -d -p 3307:3307 --privileged=true -v /usr/blog/mysql/conf/my.cnf:/etc/mysql/my.cnf -v /usr/blog/mysql/logs:/logs -v /usr/blog/mysql/data:/var/lib/mysql --name blog -e MYSQL_ROOT_PASSWORD=ZHUOHAN2022 -e TZ=Asiz/Shanghai mysql

```

mysql配置文件内容

```shell
[mysqld]
port = 3307
pid-file = /var/run/mysqld/mysqld.pid
socket = /var/run/mysqld/mysqld.sock
datadir = /var/lib/mysql
secure-file-priv= NULL
symbolic-links=0
```

### 1.3安装redis

```shell
docker pull redis //下载Redis镜像
docker run --name redis --restart=always -p 6379:6379 -d redis --requirepass "密码" //启动Redis
```

### 1.3安装RabbitMq

```shell
docker pull rabbitmq:management //下载RabbitMQ镜像
docker run --name rabbit --restart=always -p 15672:15672 -p 5672:5672 -d rabbitmq:management //启动RabbitMQ,默认guest用户，密码也是guest。
验证rabbitmq
网页输入 ip:15672
```



