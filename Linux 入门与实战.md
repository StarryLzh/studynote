# Linux 入门与实战

### Linux管道符、重定向与环境变量

**输入输出重定向**

把多个 Linux 命令适当地组合到一起，使其协同工作，以便我们更加高效地处理数据。

> 标准输入重定向（STDIN，文件描述符为 0）：默认从键盘输入，也可从其他文件或命令中输入。
>
> 标准输出重定向（STDOUT，文件描述符为 1）：默认输出到屏幕。
>
> 错误输出重定向（STDERR，文件描述符为 2）：默认输出到屏幕。

## Linux下软件安装的命令

![img](https://www.itbaizhan.com/wiki/imgs/SNAGHTML6a094b5.PNG)

**源码安装**

以源代码安装软件，每次都需要配置操作系统、配置编译参数、实际编译，最后还要依据个人喜好的方式来安装软件。这个过程很麻烦很累人。

### RPM软件包管理

![image-20211023110357301](https://www.itbaizhan.com/wiki/imgs/image-20211023110357301.png)

**RPM安装软件的默认路径**:

> **注意：**
>
> - /etc 配置文件放置目录
> - /usr/bin 一些可执行文件
> - /usr/lib 一些程序使用的动态链接库
> - /usr/share/doc 一些基本的软件使用手册与说明文件
> - /usr/share/man 一些man page档案

常用的 RPM 软件包命令

| 安装软件的命令格式              | rpm -ivh filename.rpm |
| ------------------------------- | --------------------- |
| 升级软件的命令格式              | rpm -Uvh filename.rpm |
| 卸载软件的命令格式              | rpm -e filename.rpm   |
| 查询软件描述信息的命令格式      | rpm -qpi filename.rpm |
| 列出软件文件信息的命令格式      | rpm -qpl filename.rpm |
| 查询文件属于哪个 RPM 的命令格式 | rpm -qf filename      |

> **参数说明：**
>
> - -i：install的意思，安装
> - -v：查看更详细的安装信息画面
> - -h：以安装信息栏显示安装进度

#### 软件安装

如你需要安装一个jdk，首先要到网上下载一个jdk的rpm包，如jdk-8u171-linux-x64.rpm。最简单的安装命令如下：

```
rpm -i jdk-8u171-linux-x64.rpm
```

不过，这样的参数其实无法显示安装的进度，所以通常我们执行的命令是这样：

```
rpm -ivh package-name
```

#### 卸载软件

使用rpm的卸载过程一定要由最上层往下卸载，以rp-pppoe为例，这个软件主要是依据ppp这个软件来安装的，所以当你要卸载ppp的时候，就必须先卸载rp-pppoe才行！

删除的命令非常简单，通过-e参数就可以完成。不过，很常发生软件属性依赖导致无法山洼某些软件的问题。

```
rpm -e gcc
```

> 注意：
>
> 如果删除不想检查依赖关系，加 --nodeps

### YUM

YUM可以看作是CS架构的软件，YUM的存在很好的解决了RPM的属性依赖问题。

**YUM通过依赖rpm软件包管理器,** 实现了rpm软件包管理器在功能上的扩展, 因此YUM是不能脱离rpm而独立运行的。

![img](https://www.itbaizhan.com/wiki/imgs/SNAGHTML14f128f8.PNG)

> **注意：**
>
> YUM是一个在线软件管理工具，所以使用YUM进行的操作大都是需要在联网的条件下才能正常使用。

#### YUM的配置文件

##### 容器说明

虽然yum是你在联网后就能直接使用，不过，由于你**系统的站点镜像没选择好**，会导致连接速度非常慢！所以，**这时候就需要我们去手动修改yum的设置文档了**。

##### 容器查询

首先，可以先查询一下目录yum server所使用的容器有哪些。

使用命令：yum repolist all，查询结果如下：

![clipboard.png](https://www.itbaizhan.com/wiki/imgs/bVU9Td.png)

> 如上图，只有当最右边的status为enabled该容器才算激活，
>
> /etc/yum.repos.d/里面会有多个配置文件（文件名以.repo结尾），yum会从里面逐个查找，所以里面的容器名称不能有重复。

###### 配置文件修改

打开配置文件：vi /etc/yum.repos.d/CenOS-Base.repo，内容如下

![clipboard.png](https://www.itbaizhan.com/wiki/imgs/bVU9Tn.png)

> 配置文件的说明：
>
> [base]：代表容器的名字。中括号一定要存在，里面的名称可以随意起，但不能有两个相同的容器名称，否则yum会不知道去哪里找容器相关软件列表文件。
>
> name：只是说明一下这个容器的意义而已，重要性不高。
>
> mirrorlist：列出这个容器可以使用的镜像站点，如果不想使用可以批注掉这一行。
>
> baseurl：这个最重要，因为后面接的就是容器的实际网址。mirrorlist是由yum程序自行去找镜像站点， baseurl则是指定固定的一个容器网址。
>
> enable=1：启动这个容器，默认值也为1。关闭这个容器可以设置enable=0。
>
> gpgcheck=1：指定是否需要查阅RPM文件内的数字证书。
>
> gpgkey：数字证书的公钥文件所在位置，使用默认值即可。

> **注意：**
>
> 1）yum会自动识别/etc/yum.repos.d/目录以.repo结尾的文件。
>
> 2）当我们修改了配置文件的网址却没有修改容器名称，可以会造成本机的列表与yum服务器的列表不同步，这时就需要手动来清除容器的数据了：
>
> 语法：yum clean [packages|headers|all]
>
> 参数：
>
> packages：将已下载的软件文件删除
>
> headers：将下载的软件文件头删除
>
> all：将所有容器数据都删除
>
> 例：删除已下载过的所有容器相关数据（含软件本身与列表）
>
> yum clean all

**示例1**

配置阿里yum源

```
1：安装wget
    yum install -y wget  （如果已经安装了则省略）
2：备份  /etc/yum.repos.d/CentOS-Base.repo文件
    cd /etc/yum.repos.d/
    mv CentOS-Base.repo CentOS-Base.repo.back
3：下载阿里云的Centos-7.repo文件
     wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
4：重新加载yum
    yum clean all
    yum makecache
5：检查配置的源是否是阿里的
   cat /etc/yum.repos.d/CentOS-Base.repo
```

![img](https://www.itbaizhan.com/wiki/imgs/SNAGHTML15063381.PNG)

##### YUM使用手册

![image-20211102163040572](https://www.itbaizhan.com/wiki/imgs/image-20211102163040572.png)

**小技巧：** 使用参数-y，当遇到需要等待用户输入时，这个选项会提供yes的响应，如上面的例子可以写成：

```
yum  install -y  emacs 
```