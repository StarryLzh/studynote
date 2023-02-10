# Elasticsearch

---

## 一、介绍

### 基本概念

Elasticsearch是一个**全文检索服务器**

**全文检索**是一种**非结构化数据**的**搜索方式**

* 结构化数据：指具有固定格式固定长度的数据，如数据库中的字段。

<img src="C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221211113253542.png" alt="image-20221211113253542" style="zoom: 67%;" />

* 非结构化数据：指格式和长度不固定的数据，如电商网站的商品详情。

<img src="C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221211113505079.png" alt="image-20221211113505079" style="zoom:50%;" />

结构化数据一般存入数据库，使用sql语句即可快速查询。但由于非结构化数据的数据量大且格式不固定，我们需要采用全文检索的方式进行搜索。全文检索通过建立**倒排索引**加快搜索效率。

### **倒排索引**

![image-20221211114113511](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221211114113511.png)

#### **索引**

将数据中的一部分信息提取出来，重新组织成一定的数据结构，我们可以根据该结构进行快速搜索，这样的结构称之为索引。

索引即目录，例如字典会将字的拼音提取出来做成目录，通过目录即可快速找到字的位置

索引分为**正排索引**和**倒排索引**。

##### **正排索引（正向索引）**

将文档id建立为索引，通过id快速可以快速查找数据。如数据库中的主键就会创建正排索引。

![image-20221211163657247](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221211163657247.png)

##### **倒排索引（反向索引）**

非结构化数据中我们往往会根据关键词查询数据。此时我们将数据中的关键词建立为索引，指向文档数据，这样的索引称为倒排索引。

<img src="C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221211163708894.png" alt="image-20221211163708894" style="zoom:80%;" />

创建倒排索引流程：

1. 分词
2. 存入索引库 

![image-20221212102632298](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221212102632298.png)



### Elasticsearch的出现

多年前，一个刚结婚的名叫Shay的失业开发者，跟着妻子去了伦敦，他的妻子在那里学习厨师。Shay使用全文检索工具——lucene，给他的妻子做一个食谱搜索引擎。

![image-20221212102459532](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221212102459532.png)

但Lucene的操作非常复杂，且Lucene是一个单机软件，不支持联网访问。因此 Shay基于Lucene开发了开源项目 Elasticsearch。Elasticsearch本质是一个java语言开发的web项目，我们可以通过RESTful风格的接口访问该项目内部的Lucene，从而让全文搜索变得简单。

从此以后，Elasticsearch成为了Github上最活跃的项目之一， Elastic公司围绕Elasticsearch提供商业服务，并开发新的特性。Elasticsearch将永远开源并对所有人可用。

Elasticsearch是**java语言开发的web项目**



### Elasticsearch应用场景

* 全站搜索
* 线上商城系统的搜索 
* 分析日志
* 2013年初，GitHub抛弃了Solr，采取Elasticsearch来做PB级的搜索。GitHub使用Elasticsearch搜索20TB 的数据，包括13亿文件和1300亿行代码。
* 维基百科：以Elasticsearch为基础的核心搜索架构。
* 百度：百度目前广泛使用Elasticsearch作为文本数据分析，采集百度所有服务器上的各类指标数据及用户自定义数据。目前覆盖百度内部20多个业务线（包括casio、云分析、网盟、预测、文库、直达号、钱包、风控等），单集群最大100台机器，200个ES节点，每天导入30TB+数据
* 新浪使用ES分析处理32亿条实时日志。
* 阿里使用ES构建自己的日志采集和分析体系。
* 我们可以使用Elasticsearch实现全站搜索，线上商城系统的搜索，分析日志等功能。
* 

### Elasticsearch对比Solr

Solr也是基于Lucene的一款全文搜索引擎，下面是他们的对比。

- Solr利用Zookeeper进行分布式管理，而Elasticsearch自身带有分布式协调管理功能;
- Solr支持更多格式的数据，而Elasticsearch仅支持json文件格式；
- Solr官方提供的功能更多，而Elasticsearch本身更注重于核心功能，高级功能多由第三方插件提供；
- Solr在传统的搜索应用中表现好于Elasticsearch，但在处理实时搜索应用时效率明显低于Elasticsearch。

目前Elasticsearch的市场占有率越来越高，Spring从2020年起也已经停止Spring Data Solr的维护，更多的公司使用Elasticsearch作为搜索引擎。



### Elasticsearch数据结构

文档（Document）：文档是可被查询的最小数据单元，一个 Document 就是一条数据。类似于关系型数据库中的记录的概念。

类型（Type）：具有一组共同字段的文档定义成一个类型，类似于关系型数据库中的数据表的概念。

索引（Index）：索引是多种类型文档的集合，类似于关系型数据库中的库的概念。

域（Fied）：文档由多个域组成，类似于关系型数据库中的字段的概念。

Elasticsearch跟关系型数据库中概念的对比：

| JAVA  | 项目     | 实体类 | 对象     | 属性   |
| ----- | -------- | ------ | -------- | ------ |
| ES    | Index    | Type   | Document | Filed  |
| Mysql | Database | Table  | Row      | Column |

> 注：ES7.X之后删除了type的概念，一个索引不会代表一个库，而是代表一张表。我们课程中使用ES7.17，所以目前的ES中概念对比为：

| JAVA  | 项目     | 实体类 | 对象     | 属性   |
| ----- | -------- | ------ | -------- | ------ |
| ES    |          | Index  | Document | Filed  |
| Mysql | Database | Table  | Row      | Column |



## 二、Elasticsearch安装

### 1、安装ES服务

#### 准备工作

1. 准备一台搭载有CentOS7系统的虚拟机，使用XShell连接虚拟机

2. 关闭防火墙，方便访问ES

   ```bash
   #关闭防火墙：
   systemctl stop firewalld.service
   
   #禁止防火墙自启动：
   systemctl disable firewalld.service
   ```

3. 配置最大可创建文件数大小

   ```bash
   #打开系统文件：
   vim /etc/sysctl.conf
   
   #添加以下配置：
   vm.max_map_count=655360
   
   #配置生效：
   sysctl -p
   
   #修改/etc/security/limits.conf，在末尾添加
   
   *  soft nofile 65536
   *  hard nofile 65536
   *  soft nproc  4096
   *  hard nproc  4096
   ```

4. 由于ES不能以root用户运行，我们需要创建一个非root用户，此处创建一个名为es的用户：

   ```bash
   #创建用户：
   useradd es
   ```

#### 安装服务

1. 使用rz命令将linux版的ES上传至虚拟机

2. 解压ES

   ```bash
   #解压：
   tar -zxvf elasticsearch-7.17.0-linux-x86_64.tar.gz
   
   #重命名：
   mv elasticsearch-7.17.0 elasticsearch1
   
   #移动文件夹：
   mv elasticsearch1 /usr/local/
   
   #es用户取得该文件夹权限：
   chown -R es:es /usr/local/elasticsearch1
   ```

3. 启动ES服务：

   ```bash
   #切换为es用户：
   su es
   
   #进入ES安装文件夹：
   cd /usr/local/elasticsearch1/bin/
   
   #启动ES服务：
   ./elasticsearch
   
   #查询ES服务是否启动成功
   curl 127.0.0.1:9200
   ```

### 2、安装kibana

Kibana是一款开源的数据分析和可视化平台，设计用于和Elasticsearch协作。我们可以使用Kibana对Elasticsearch索引中的数据进行搜索、查看、交互操作。

1. 使用rz工具将Kibana压缩文件上传到Linux虚拟机

2. 解压

   ```bash
   tar -zxvf kibana-7.17.0-linux-x86_64.tar.gz -C /usr/local/
   ```

3. 修改配置

   ```bash
   # 进入Kibana解压路径
   cd /usr/local/kibana-7.17.0-linux-x86_64/config
   
   # 修改配置文件
   vim kibana.yml
   
   # 加入以下内容
   # kibana主机IP
   server.host: "虚拟机IP"
   # Elasticsearch路径
   elasticsearch.hosts: ["http://127.0.0.1:9200"]
   
   ```

4. 启动：

   kibana不能以root用户运行，我们给es用户设置kibana目录的权限，并使用es用户运行kibana

   ```bash
   # 给es用户设置kibana目录权限
   chown -R es:es /usr/local/kibana-7.17.0-linux-x86_64/
   
   # 切换为es用户
   su es
   
   # 启动kibana
   cd /usr/local/kibana-7.17.0-linux-x86_64/bin/
   ./kibana
   ```

5. 访问kibana：http://虚拟机IP:5601
6. 点击`Management` => `Index Management`可以查看es索引信息。

### 3、Docker安装

#### 3.1安装Elasticsearch

1. 在Centos7中安装docker

   ```bash
   #安装Docker
   curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
   
   # 启动docker
   systemctl start docker
   ```

2. 拉取镜像

   ```
   docker pull elasticsearch:7.17.0
   ```

3. 启动容器

   ```bash
   # docker容器间建立通信
   docker network create elastic
   
   # 创建es容器
   docker run --restart=always -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms512m -Xmx512m" --name='elasticsearch' --net elastic --cpuset-cpus="1" -m 1G -d elasticsearch:7.17.0
   ```

#### 3.2安装Kibana

1. 拉取镜像

   ```bash
   docker pull kibana:7.17.0
   ```

2. 启动容器

   ```bash
   docker run --name kibana --net elastic --link elasticsearch:elasticsearch -p 5601:5601 -d kibana:7.17.0
   ```

3. 访问kibana：http://虚拟机IP:5601



## 三、Elasticsearch常用操作

Elasticsearch是使用#[RESTful](https://javaguide.cn/system-design/basis/RESTfulAPI.html)风格的http请求访问操作的，请求参数和返回值都是Json格式的，我们可以使用kibana发送http请求操作ES。

### 1、索引操作

#### 创建没有结构的索引

路径：ip地址:端口号/索引名

> 注：在kibana中所有的请求都会省略 ip地址:端口号 ，之后的路径我们省略写 ip地址:端口号

请求方式：PUT（新增索引）

##### 为索引添加结构

```json
PUT /student(索引名)
POST /student/_mapping
{
  "properties":{
    #"域名1":{
 	  #"type":域的类型,
 	  #"store":是否存储,
      #"index":是否创建索引,
      #"analyzer":分词器
    #},
    "id":{
      "type":"integer"
    },
    "name":{
      "type":"text"
    },
    "age":{
      "type":"integer"
    }
  }
}}
}
```

#### 创建有结构的索引

```json
PUT /student1(索引名)
{
  "mappings": {
    "properties": {
      "id":{
        "type": "integer"
      },
      "name":{
        "type": "text"
      },
      "age":{
        "type": "integer"
      }
    }
  }
}
```

#### 删除索引

```json
DELETE /student1(索引名)
```

### 2、文档操作

即对数据进行增删改查

#### 新增/修改文档

```json
POST /student/_doc/1
#POST /索引/_doc/[id值] 此id值在查询时为 String类型
{
    #"field名":field值
	#此ID和查询id意义不同，上为属性_id,一个为字段fild域id值
    "id":1,
    "name":"bz",
    "age":10
}
```

>注：id值不写时自动生成文档id，id和已有id重复时修改文档

#### * 查询文档

```json
#GET /索引/_doc/[id值]
GET /student/_doc/1 
```

#### 删除文档

```json	
DELETE /索引/_doc/[id值]
```

#### * 根据id批量查询文档 

```json
GET /索引/_mget
{
  #docs值
  "docs":[
     {"_id":id值},
     {"_id":id值}
   ] 
}
```

#### * 查询所有文档

匹配内容没有**即匹配所有**

```json
GET /索引/_search
{
   "query": {
     "match_all": {}
   }
}
```

#### 修改文档部分字段

```json
POST /索引/_doc/id值/_update
{ 
  #doc值
  "doc":{ 
    域名:值
    } 
}
```

>注：
>
>Elasticsearch**执行删除操作时**，ES先**标记文档为deleted状态**，而不是直接物理删除。当ES存储空间不足或工作空闲时，才会执行物理删除操作。
>
>Elasticsearch**执行修改操作时**，ES不会真的修改Document中的数据，而是**标记ES中原有的文档为deleted状态**，再创建一个新的文档来存储数据。

### 3、域的属性

#### index #[参见索引操作]

该域是否创建索引。只有值设置为true，才能**根据该域的关键词查询文档**。

```json
PUT /student(索引名)、
#无结构
POST /student/_mapping
{
  "properties":{
    "域名1":{
 	  "type":域的类型,
 	  "store":是否存储,单独存储
      "index":是否创建索引,建立倒排索引
      "analyzer":分词器
    }
}
#有结构新建
PUT /student1(索引名)
{
  "mappings": {
    "properties": {
        "域名1":{
            "type":域的类型,
 	  		"store":是否存储,单独存储
      		"index":是否创建索引,建立倒排索引
      		"analyzer":分词器
    		}
    }
  }
```

即这个域是否创建倒排索引。

```json
// 根据关键词查询文档
GET /索引名/_search
{
    "query":{
    "term":{ 
            搜索字段: 关键字
        } 
   }
}
```

#### type (类型)

域的类型

| 核心类型       | 具体类型                   |
| -------------- | -------------------------- |
| 字符串类型     | text                       |
| 整数类型       | long, integer, short, byte |
| 浮点类型       | double, float              |
| 日期类型       | date                       |
| 布尔类型       | boolean                    |
| 数组类型       | array                      |
| 对象类型       | object                     |
| 不分词的字符串 | keyword                    |

#### Store

是否单独存储。如果设置为true，**则该域能够单独查询**。

```json
// 单独查询某个域：
GET /索引名/_search
{
 "stored_fields": ["域名"]
}
```



## 四、分词器

### 1、默认分词器

ES文档的数据拆分成一个个有完整含义的关键词，并将关键词与文档对应，这样就可以通过关键词查询文档。要想正确的分词，需要选择合适的分词器。

**standard analyzer**：Elasticsearch默认分词器，根据**空格和标点符号对英文进行分词**，会进行**单词的大小写转换**。

> 默认分词器是英文分词器，对中文的分词是一字一词。

- 查看分词效果

```json
GET /_analyze
{
    "text":测试语句,
    "analyzer":分词器
}
```



### 2、Ik分词器

IKAnalyzer是一个开源的，基于java语言开发的轻量级的中文分词工具包。提供了两种分词算法：

- ik_smart：最少切分
- ik_max_word：最细粒度划分

#### 安装IK分词器

1. 关闭es服务

   ```  shell
   su root
   #查找进程
   ps -ef | grep elastic
   
   kill -9 [端口号]
   ```

2. 使用rz命令将ik分词器上传至虚拟机

   > 注：ik分词器的版本要和es版本保持一致。

3. 解压ik分词器到elasticsearch的plugins目录下

   ```shell
   unzip elasticsearch-analysis-ik-7.17.0.zip -d /usr/local/elasticsearch1/plugins/analysis-ik
   ```

4. 启动ES服务

   ```shell
   su es
   
   #进入ES安装文件夹：
   cd /usr/local/elasticsearch1/bin/
   
   #启动ES服务：
   ./elasticsearch -d
   ```

##### 测试分词器效果

```json
GET /_analyze
{
    "text":"测试语句", 
    "analyzer":"ik_smart/ik_max_word"
}
```

#### IK分词器词典

IK分词器根据词典进行分词，词典文件在IK分词器的config目录中。

- main.dic：IK中内置的词典。记录了IK统计的所有中文单词。

- IKAnalyzer.cfg.xml：用于配置自定义词库。

  ```xml
  <properties>
      <comment>IK Analyzer 扩展配置</comment>
      <!--用户可以在这里配置自己的扩展字典 -->
      <entry key="ext_dict">ext_dict.dic</entry>
       <!--用户可以在这里配置自己的扩展停止词字典-->
      <entry key="ext_stopwords">ext_stopwords.dic</entry>
      <!--用户可以在这里配置远程扩展字典 -->
      <!-- <entry key="remote_ext_dict">words_location</entry> -->
      <!--用户可以在这里配置远程扩展停止词字典-->
      <!-- <entry key="remote_ext_stopwords">words_location</entry> -->
  </properties>
  ```

  ```shell
  #重启es和kibana
  netstate -anp | grep 5601
  
  kill -9 [端口号]
  
  ps -ef | grep elastic
  
  kill -9 [端口号]
  #使用非root用户
  ```

### 3、拼音分词器

拼音分词器可以将中文分成对应的全拼，全拼首字母等。

#### 安装拼音分词器

1. 关闭es服务

2. 使用rz命令将拼音分词器上传至虚拟机

   > 注：拼音分词器的版本要和es版本保持一致。

3. 解压ik分词器到elasticsearch的plugins目录下

   ```shell
   unzip elasticsearch-analysis-pinyin-7.17.0.zip -d /usr/local/elasticsearch1/plugins/analysis-pinyin
   ```

4. 启动ES服务

   ```shell
   su es
   
   #进入ES安装文件夹：
   cd /usr/local/elasticsearch1/bin/
   
   #启动ES服务：
   ./elasticsearch
   ```

#### 测试分词效果

```json
GET /_analyze
{
    "text":测试语句, 
    "analyzer":pinyin
}
```

### 4、* 自定义分词器

真实开发中我们往往需要对一段内容既进行文字分词，又进行拼音分词，此时我们需要自定义ik+pinyin分词器。

#### 创建自定义分词器

- 在创建索引时自定义分词器

```json
PUT /索引名
{
  "settings" : {
    "analysis" : {
      "analyzer" : {
        "ik_pinyin" : { //自定义分词器名
          "tokenizer":"ik_max_word", // 基本分词器
          "filter":"pinyin_filter" // 配置分词器过滤
         }
       },
      "filter" : { // 分词器过滤时配置另一个分词器，相当于同时使用两个分词器
        "pinyin_filter" : { 
          "type" : "pinyin", // 另一个分词器
          // 拼音分词器的配置
          "keep_separate_first_letter" : false, // 是否分词每个字的首字母
          "keep_full_pinyin" : true, // 是否分词全拼
          "keep_original" : true, // 是否保留原始输入
          "remove_duplicated_term" : true // 是否删除重复项
         }
       }
     }
   },
  "mappings":{
    "properties":{
      "域名1":{
        "type":域的类型,
        "store":是否单独存储,
        "index":是否创建索引,
             "analyzer":分词器
       },
      "域名2":{ 
        ...
       }
     }
   }
}

```

#### 测试分词效果

```json
GET /索引/_analyze
{
 "text": "你好百战程序员",
 "analyzer": "ik_pinyin"
}
```



## 五、Elasticsearch搜索文档

### 1、准备工作

Elasticsearch提供了全面的文档搜索方式，在学习前我们添加一些文档数据

```json
#新建索引
PUT /students
{
  "mappings":{
    "properties":{
      "id": {
            "type": "integer",
        	"index": true
       },
         "name": {
            "type": "text",
            "store": true,
        	"index": true,
            "analyzer": "ik_smart"
         },
      "info": {
            "type": "text",
            "store": true,
        	"index": true,
            "analyzer": "ik_smart"
         }
     }
   }
}

#添加文档数据
POST /students/_doc/
{
 "id":1,
 "name":"百战程序员",
 "info":"I love baizhan"
}


POST /students/_doc/
{
 "id":2,
 "name":"美羊羊",
 "info":"美羊羊是羊村最漂亮的人"
}


POST /students/_doc/
{
 "id":3,
 "name":"懒羊羊",
 "info":"懒羊羊的成绩不是很好"
}


POST /students/_doc/
{
 "id":4,
 "name":"小灰灰",
 "info":"小灰灰的年纪比较小"
}


POST /students/_doc/
{
 "id":5,
 "name":"沸羊羊",
 "info":"沸羊羊喜欢美羊羊"
}


POST /students/_doc/
{
 "id":6,
 "name":"灰太狼",
 "info":"灰太狼是小灰灰的父亲，每次都会说我一定会回来的"
}
```

#### 参见根据

id批量查询文档

- 文档搜索

```json
GET /索引/_search
{
    "query":{
    搜索方式:搜索参数
   }
}
```

### 2、*  搜索方式

#### match_all：查询所有文档

```json
{
    "query":{
    "match_all":{}
   }
}
```

#### match：全文检索。

将查询条件分词后再进行搜索。

```json
{
    "query":{
    "match":{
      搜索字段:搜索条件
     }
   }
}
```

> 注：在搜索时关键词有可能会输入错误，ES搜索提供了自动纠错功能，即ES的模糊查询。使用match方式可以实现模糊查询。模糊查询对中文的支持效果一般，我们使用英文数据**测试模糊查询**。

```json
{
    "query": {
        "match": {
            "域名": {
            "query": 搜索条件,
            "fuzziness": 最多错误字符数,不能超过2,即最多纠错两个字符
            }
        }
    }
}
```

#### range：范围搜索。

对数字类型的字段进行范围搜索

> **gt/lt**:大于/小于
> **gte/lte**:大于等于/小于等于

```json
{
    "query":{
    "range":{
      搜索字段:{ 
        "gte":最小值,
        "lte":最大值
       } 
     }
   }
}
```

#### match_phrase：短语检索。

只支持关键词

搜索条件**不做任何分词解析**，在搜索字段对应的倒排索引中精确匹配。

```json
{
    "query":{
    "match_phrase":{
      搜索字段:搜索条件
     }
   }
}
```

#### term/terms：单词/词组搜索。

搜索条件不做任何分词解析，在搜索字段对应的倒排索引中精确匹配

与phrase基本一样

```json
{
    "query":{
    "term":{ 
            搜索字段: 搜索条件
     }
   }
}


{
    "query":{
    "terms":{ 
            搜索字段: [搜索条件1,搜索条件2]
     }
   }
}
```

### 3、复合搜索

``query``：最外层

`` bool``：复合搜索

``must``：：必须满足

``should``：多个条件有任意一个满足即可

``must_not``:必须不满足的条件

**模板**

```json
GET /索引/_search
{ 
    "query": { 
    "bool": { 
          // 必须满足的条件 类似and 
          "must": [ 
                    搜索方式:搜索参数,
                    搜索方式:搜索参数
           ],
          // 多个条件有任意一个满足即可 类似or
          "should": [
                    搜索方式:搜索参数,
                   搜索方式:搜索参数
               ],
                // 必须不满足的条件 类似not
           "must_not":[
               搜索方式:搜索参数,
               搜索方式:搜索参数
           ]
       } 
   } 
}
```

>温馨提示： 三种逻辑要求里的``[ ]``内搜索方式：搜索参数，要用``{ }``包含

```json
 "must":[
                {	
                    //关键词搜索
                    "term": {
                        //成绩字段
                        "info": "成绩"
                	}
                }，
                {
                	//范围搜索
                	"range":{
                        "id":{
                        "gte": 1,
                        "lte": 3
                        }
                    }
                }
            ]
```



**实操**

```json
GET /students/_search
{
    "query": {
        "bool": {
            "must":[
                {	//一种搜索方式一个花括号
                    //关键词搜索
                    "term": {
                        //成绩字段
                        "info": "成绩"
                	}
                }，
                {
                	//范围搜索
                	"range":{
                        "id":{
                        "gte": 1,
                        "lte": 3
                        }
                    }
                }
            ],
            "should":[
                {
                    "term":{
                        "info": "成ji"
                    }
                }
            ],
            "must_not":[
                
            ]
        }
    }
}
```



### 4、结果排序

ES中默认使用相关度分数实现排序，可以通过搜索语法定制化排序。

```json
GET /索引/_search
{ 
  "query": 搜索条件,
  "sort": [
       {
           "字段1":{
               "order":"asc"
           } 
       },
       { 
           "字段2":{ 
               "order":"desc" 
           } 
       }
   ] 
}
```

>由于ES对text类型字段数据会做分词处理，使用哪一个单词做排序都是不合理的，所以 **ES中默认不允许对text类型的字段做排序**。如果需要使用字符串做结果排序，可以使用 keyword类型的字段作为排序依据，因为keyword字段不做分词处理。

### 5、分页查询

```json
GET /索引/_search
{ 
    "query": 搜索条件,
    "from": 起始下标,
    "size": 查询记录数
}
```



### 6、高亮查询

在进行关键字搜索时，搜索出的内容中的关键字会显示不同的颜色，称之为高亮。

为什么在网页中关键字会显示不同的颜色，我们通过开发者工具查看网页源码：

![](https://www.itbaizhan.com/wiki/imgs/image-20210531102710710.png?v=1.0.0)

我们**可以在关键字左右加入标签字符串**，数据传入前端即可完成高亮显示，ES可以对查询出的内容中关键字部分进行标签和样式的设置。

```json
GET /索引/_search
{ 
    "query":搜索条件,
    "highlight":{
        "fields": { 
         "高亮显示的字段名": {
        // 返回高亮数据的最大长度
             "fragment_size":100,
                // 返回结果最多可以包含几段不连续的文字
             "number_of_fragments":5
         } 
     },
    "pre_tags":["前缀"], 
    "post_tags":["后缀"]
   } 
}
```

**实操**

```json
GET /索引/_search
{ 
    "query":{
        "match":{
            "info":"我喜欢成绩好的同学"
        }
    },
    "highlight":{
        "fields": { 
         "info": {
        // 返回高亮数据的最大长度
             "fragment_size":100,
                // 返回结果最多可以包含几段不连续的文字
             "number_of_fragments":5
         } 
     },
        //前后添加<em>标签
    "pre_tags":["<em>"], 
    "post_tags":["</em>"]
   } 
}
```



### 7、SQL查询

在ES7之后，支持SQL语句查询文档：

```json
GET /_sql?format=txt
{
    "query": SQL语句
}
```

> 开源版本的ES并不支持通过Java操作SQL进行查询，如果需要操作 SQL查询，则需要氪金（购买白金版）



## 六、原生JAVA操作ES

### 1、搭建项目

原生JAVA可以对ES的索引和文档进行操作，但操作较复杂，我们了解即可。

1. 创建maven项目

2. maven项目引入以下依赖：

   ```xml
   <dependencies>
     <dependency>
       <groupId>org.elasticsearch</groupId>
       <artifactId>elasticsearch</artifactId>
       <version>7.17.0</version>
     </dependency>
     <dependency>
       <groupId>org.elasticsearch.client</groupId>
       <artifactId>elasticsearch-rest-high-level-client</artifactId>
       <version>7.17.0</version>
     </dependency>
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
     </dependency>
   </dependencies>
   
   ```

### 2、索引操作

#### 创建空索引

```java
// 创建空索引
@Test
public void createIndex() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  CreateIndexRequest request = new CreateIndexRequest("student");


  // 3.发送请求
  CreateIndexResponse response = client.indices().create(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.index());


  // 5.关闭客户端
  client.close();
}
```

> 外部无法访问ES的解决方案：
>
> 打开Elasticsearch安装路径下config目录下的elasticsearch.yml文件，加入如下配置：
>
> ```yaml
> discovery.seed_hosts: ["host1"]
> network.host: 0.0.0.0
> ```
>
> 重新启动ES即可。
>
> PS：如果修改配置文件后，启动报错`max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]`，解决方案如下：
>
> 编辑 /etc/security/limits.conf，添加以下内容；
>
> ```
> * soft nofile 65536
> * hard nofile 65536
> es soft nproc 65536
> es hard nproc 65536
> ```
>
> 修改后**重新登录**es用户，启动ES即可

#### 给索引添加结构

```java
@Test
public void mappingIndex() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  PutMappingRequest request = new PutMappingRequest("student");
  request.source("{\n" +
          "  \"properties\":{\n" +
          "   \"id\":{\n" +
          "    \"type\":\"integer\"\n" +
          "   },\n" +
          "   \"name\":{\n" +
          "    \"type\":\"text\"\n" +
          "   },\n" +
          "   \"age\":{\n" +
          "    \"type\":\"integer\"\n" +
          "   }\n" +
          "  }\n" +
          "}", XContentType.JSON);


  // 3.发送请求
  AcknowledgedResponse response = client.indices().putMapping(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.isAcknowledged());


  // 5.关闭客户端
  client.close();
}
```

#### 删除索引

```java
// 删除索引
@Test
public void deleteIndex() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  DeleteIndexRequest request = new DeleteIndexRequest("student");


  // 3.发送请求
  AcknowledgedResponse response = client.indices().delete(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.isAcknowledged());


  // 5.关闭客户端
  client.close();
}
```

### 3、文档操作

#### 新增&修改文档

```java
@Test
public void addDocument() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  IndexRequest request = new IndexRequest("student").id("1");
  request.source(XContentFactory.jsonBuilder()
          .startObject()
          .field("id", 1)
          .field("name", "i love baizhan")
          .field("age", 20)
          .endObject());


  // 3.发送请求
  IndexResponse response = client.index(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.status());


  // 5.关闭客户端
  client.close();
}
```

#### 根据id查询文档

```java
// 根据id查询文档
@Test
public void findByIdDocument() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  GetRequest request = new GetRequest("student", "1");


  // 3.发送请求
  GetResponse response = client.get(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.getSourceAsString());


  // 5.关闭客户端
  client.close();
}
```

#### 删除文档

```java
// 删除文档
@Test
public void DeleteDocument() throws IOException {
  // 1.创建客户端对象，连接ES
  RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(new HttpHost("192.168.66.100", 9200, "http")));


  // 2.创建请求对象
  DeleteRequest request = new DeleteRequest("student", "1");


  // 3.发送请求
  DeleteResponse response = client.delete(request, RequestOptions.DEFAULT);


  // 4.操作响应结果
  System.out.println(response.status());


  // 5.关闭客户端
  client.close();
}
```

### 4、搜索操作

#### 搜索所有文档

```java
@Test
public void queryAllDocument() throws IOException {
  // 创建客户端对象，链接ES
  RestHighLevelClient client = new RestHighLevelClient(
    RestClient.builder(new HttpHost("192.168.1.58",9200,"http")));


  // 创建搜索条件
  SearchSourceBuilder searchSourceBuilder = new SearchSourceBuilder();
  searchSourceBuilder.query(QueryBuilders.matchAllQuery());


  // 创建请求对象
  SearchRequest request = new SearchRequest("student").source(searchSourceBuilder);


  // 发送请求
  SearchResponse response = client.search(request, RequestOptions.DEFAULT);


  // 输出返回结果
  for (SearchHit hit:response.getHits()) {
    System.out.println(hit.getSourceAsString());
   }


  // 关闭客户端
  client.close();
}

```

## 七、Spring Data ES

### 1、入门案例

#### 项目搭建

Spring Data ElasticSearch是Spring对原生JAVA操作Elasticsearch封装之后的产物。它通过对原生API的封装，使得JAVA程序员可以简单的对Elasticsearch进行操作。

1. 创建SpringBoot项目，加入Spring Data Elasticsearch起步依赖：

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
   </dependency>
   ```

2. 编写配置文件：

   ```yaml
   pring:
    elasticsearch:
     uris: http://192.168.1.100:9200
   ```

   此时Spring Data ElasticSearch项目已经搭建完成。

#### 创建实体类

一个实体类的所有对象都会存入ES的一个索引中，所以我们在创建实体类时关联ES索引。

```java
@Document(indexName = "product",createIndex = true)
@Data
public class Product {
  @Id
  @Field(type = FieldType.Integer,store = true,index = true)
  private Integer id;
  @Field(type = FieldType.Text,store = true,index = true,analyzer = "ik_max_word",searchAnalyzer = "ik_max_word")
  private String productName;
  @Field(type = FieldType.Text,store = true,index = true,analyzer = "ik_max_word",searchAnalyzer = "ik_max_word")
  private String productDesc;
}
```

> @Document：标记在类上，标记实体类为文档对象，一般有如下属性：
>
> indexName：对应索引的名称
>
> createIndex：是否自动创建索引
>
> @Id：标记在成员变量上，标记一个字段为主键，该字段的值会同步到ES该文档的id值。
>
> @Field：标记在成员变量上，标记为文档中的域，一般有如下属性：
>
> type：域的类型
>
> index：是否创建索引，默认是 true
>
> store：是否单独存储，默认是 false
>
> analyzer：分词器
>
> searchAnalyzer：搜索时的分词器

#### 创建Repository接口

创建Repository接口继承ElasticsearchRepository，该接口提供了文档的增删改查方法

```java
//泛型第一个参数为 实体类 即 索引类型 ，第二个为主键类型 即 域 类型
public interface ProductRepository extends ElasticsearchRepository<Product,Integer> {
    
}
```

#### 测试方法

>温馨提示
>
>```shell
>springboot版本号不一致有bug
>
>Error:(5, 43) java: 无法访问org.springframework.data.annotation.Id
>  错误的类文件: /F:/IdeaProjects/repository/org/springframework/data/spring-data-commons/3.0.0/spring-data-commons-3.0.0.jar!/org/springframework/data/annotation/Id.class
>    类文件具有错误的版本 61.0, 应为 55.0
>    请删除该文件或确保该文件位于正确的类路径子目录中。
>```
>
>实测：把pom内容更改为
>
>```xml
>    <parent>
>        <groupId>org.springframework.boot</groupId>
>        <artifactId>spring-boot-starter-parent</artifactId>
>        <version>2.6.3</version>
>        <relativePath/> <!-- lookup parent from repository -->
>    </parent>
>```

编写测试类，注入Repository接口并测试Repository接口的增删改查方法

```java
@SpringBootTest
public class ProductRepositoryTest {
  @Autowired
  private ProductRepository repository;


  @Test
  public void addDocument(){
    Product product = new Product(1, "iphone30", "iphone30是苹果最新手机");
    repository.save(product);
   }


  @Test
  public void updateDocument(){
    Product product = new Product(1, "iphone31", "iphone31是苹果最新手机");
    repository.save(product);
   }


  @Test
  public void findAllDocument(){
    Iterable<Product> all = repository.findAll();
    for (Product product : all) {
      System.out.println(product);
     }
   }


  @Test
  public void findDocumentById(){
    Optional<Product> product = repository.findById(1);
    System.out.println(product.get());
   }


  @Test
  public void deleteDocument(){
    repository.deleteById(1);
   }
}
```

>​	**在Spring Data ElasticSearch中**
>
>​	ElasticsearchRepository提供的保存方法为save(),
>
>1. 在新增和更新索引数据时一致：A a = new 实体类，repository.save(a)；
>2. 查找
>   1. 查找所有：repository.findAll();   注意返回值
>   2. 查询byID：repository.findById(1); 注意返回值
>3. 删除：repository.deleteById(1);

### 2、查询方式

接下来我们讲解SpringDataES支持的查询方式，首先准备一些文档数据：

```java
// 添加一些数据
repository.save(new Product(2, "三体1", "三体1是优秀的科幻小说"));
repository.save(new Product(3, "三体2", "三体2是优秀的科幻小说"));
repository.save(new Product(4, "三体3", "三体3是优秀的科幻小说"));
repository.save(new Product(5, "elasticsearch", "elasticsearch是基于lucene开发的优秀的搜索引擎"));
```

---

#### 使用Repository继承的方法查询文档

该方式我们之前已经讲解过了

---

#### 使用DSL语句查询文档

ES通过json类型的请求体查询文档，方法如下：

```json
GET /索引/_search
{
  "query":{
    搜索方式:搜索参数
   }
}

/*
	即此部分
	{
    搜索方式:搜索参数
   }
*/
```

**query后的json对象**称为DSL语句，我们可以在**接口方法上**使用**@Query**注解自定义DSL语句查询。

>**DSL语句中的 ?0 为参数占位符**

```java
@Query("{" +
    "   \"match\": {" +
    "    \"productDesc\": \"?0\"" +
    "   }" +
    "  }")
List<Product> findByProductDescMatch(String keyword);

//参考五-2-2的全文搜索
@Query("{" +
    " \"match\": {" +
    "  \"productDesc\": {" +
    "   \"query\": \"?0\"," +
    "   \"fuzziness\": 1" +
    "  }" +
    " }" +
    "}")
List<Product> findByProductDescFuzzy(String keyword);
```

---

#### 按照规则命名方法进行查询

- 只需在Repository接口中按照SpringDataES的规则命名方法，该方法就能完成相应的查询。
- 规则：查询方法以findBy开头，涉及查询条件时，条件的属性用条件关键字连接。

| 关键字  | 命名规则             | 解释                        | 示例                 |
| ------- | -------------------- | --------------------------- | -------------------- |
| and     | ﬁndByField1AndField2 | 根据Field1和Field2 获得数据 | ﬁndByTitleAndContent |
| or      | ﬁndByField1OrField2  | 根据Field1或Field2 获得数据 | ﬁndByTitleOrContent  |
| is      | ﬁndByField           | 根据Field获得数据           | ﬁndByTitle           |
| not     | ﬁndByFieldNot        | 根据Field获得补集数据       | ﬁndByTitleNot        |
| between | ﬁndByFieldBetween    | 获得指定范围的数据          | ﬁndByPriceBetween    |

```java
List<Product> findByProductName(String productName);
List<Product> findByProductNameOrProductDesc(String productName,String productDesc);
List<Product> findByIdBetween(Integer startId,Integer endId);

```



### 3、分页查询

<img src="C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221225104034906.png" alt="image-20221225104034906" style="zoom:60%;" />

使用继承或自定义的方法时，**在方法中添加Pageable类型的参数**，返回值为Page类型即可进行分页查询。 

> 直接**使用对象PageRequest来实例化**Pageable对象

```java
// 测试继承的方法：
@Test
public void testFindPage(){
  // 参数1：页数，参数2：每页条数
  Pageable pageable = PageRequest.of(1, 3);
  Page<Product> page = repository.findAll(pageable);
  System.out.println("总条数"+page.getTotalElements());
  System.out.println("总页数"+page.getTotalPages());
  System.out.println("数据"+page.getContent());
}

// 自定义方法
Page<Product> findByProductDesc(String productDesc, Pageable pageable);

// 测试自定义方法
@Test
public void testFindPage2(){
  Pageable pageable = PageRequest.of(1, 2);
  Page<Product> page = repository.findByProductDescMatch("我喜欢三体", pageable);
  System.out.println("总条数"+page.getTotalElements());
  System.out.println("总页数"+page.getTotalPages());
  System.out.println("数据"+page.getContent());
}
```

### 4、结果排序

使用继承或自定义的方法时，在**方法中添加Sort类型的参数**即可进行结果排序。

> 在**PageRequest.of( start, size, sort)** 中添加排序条件Sort对象

```java
// 结果排序
@Test
public void testFindSort(){
  Sort sort = Sort.by(Sort.Direction.DESC, "id");
  Iterable<Product> all = repository.findAll(sort);
  for (Product product : all) {
    System.out.println(product);
   }
}

// 测试分页加排序
@Test
public void testFindPage2(){
  Sort sort = Sort.by(Sort.Direction.DESC,"id");
    //在PageRequest.of中添加排序条件Sort对象
  Pageable pageable = PageRequest.of(0, 2,sort);
  Page<Product> page = repository.findByProductDescMatch("我喜欢三体", pageable);
  System.out.println("总条数"+page.getTotalElements());
  System.out.println("总页数"+page.getTotalPages());
  System.out.println("数据"+page.getContent());
}
```

### 5、template工具类

> template 和repository区别就是 
>
> template 是通用需要指定操作 实体类的 Class
>
> repository是定义接口 继承类 且指定泛型

<img src="C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221225104950349.png" alt="image-20221225104950349" style="zoom:67%;" />

SpringDataElasticsearch提供了一个工具类ElasticsearchRestTemplate，我们使用该类对象也能对ES进行操作。

#### 操作索引

其实template主要操作文档，但是它还是能**获得**操作索引的对象

* template.indexOps( )：获得操作索引的对象
* indexOperations.create()：创建索引
* indexOperations.delete()：删除索引

> 测试时记得关闭Product实体类的 自动创建索引 注解
>
> 创建索引,注：该方法无法设置索引结构，不推荐使用

```java
@SpringBootTest
public class TemplateTest1 {
   
  //注入工具类
  @Autowired
  private ElasticsearchRestTemplate template;

  // 新增索引
  @Test
  public void addIndex(){
      
    // 获得索引操作对象
    IndexOperations indexOperations = template.indexOps(Product.class);
    // 创建索引,注：该方法无法设置索引结构，不推荐使用
    //测试时记得关闭Product实体类的 自动创建索引 注解 
    indexOperations.create();
   }
    

  // 删除索引
  @Test
  public void delIndex(){
    // 获得索引操作对象
    IndexOperations indexOperations = template.indexOps(Product.class);
    // 删除索引
    indexOperations.delete();
   }
}
```

#### 增删改文档

template操作文档的常用方法：

- save()：新增/修改文档
- delete()：删除文档

```java
// 新增/修改文档
@Test
public void addDocument(){
  Product product = new Product(7, "es1", "es是一款优秀的搜索引擎");
  template.save(product);
}


// 删除文档
@Test
public void delDocument(){
  template.delete("7",Product.class);
}
```

#### 查询文档

* QueryBuilders.matchQuery( ) ;
* new NativeSearchQueryBuilder().withQuery(builder).build();
* template.search(query, Product.class)

template的search方法可以查询文档：

```java
SearchHits<T> search(Query query, Class<T> clazz)
//查询文档，query是查询条件对象，clazz是结果类型。
```

用法如下：

```java
// 查询文档
@Test
public void searchDocument(){
  // 1.确定查询方式
  //     MatchAllQueryBuilder builder = QueryBuilders.matchAllQuery();
  //     TermQueryBuilder builder = QueryBuilders.termQuery("productDesc", "手机");
  MatchQueryBuilder builder = QueryBuilders.matchQuery("productDesc", "我喜欢看科幻小说");

  // 2.构建查询条件
  NativeSearchQuery query = new NativeSearchQueryBuilder().withQuery(builder).build();

  // 3.查询
  SearchHits<Product> result = template.search(query, Product.class);

  // 4.处理查询结果
  for (SearchHit<Product> productSearchHit : result) {
    Product product = productSearchHit.getContent();
    System.out.println(product);
   }
}
```

#### 复杂条件查询（动态查询）

*  BoolQueryBuilde bulder = QueryBuilders.boolQuery() ：确定查询方式 复杂查询
*  MatchAllQueryBuilder matchAllQueryBuilder = 根据查询传入条件查询
  *  QueryBuilders.matchAllQuery( )；
  *  QueryBuilders.matchQuery("productName", productName）+ QueryBuilders.matchQuery("productDesc", productDesc)
* builder.must(matchAllQueryBuilder)；

```java
@Test
public void searchDocument2() {
  //     String productName = "elasticsearch";
  //     String productDesc = "优秀";
  String productName = null;
  String productDesc = null;

  // 1.确定查询方式
  BoolQueryBuilder builder = QueryBuilders.boolQuery();
  // 如果没有传入参数，查询所有
  if (productName == null && productDesc == null) {
    MatchAllQueryBuilder matchAllQueryBuilder = QueryBuilders.matchAllQuery();
    builder.must(matchAllQueryBuilder);
   } else {
    if (productName != null && productName.length() > 0) {
      MatchQueryBuilder queryBuilder1 = QueryBuilders.matchQuery("productName", productName);
      builder.must(queryBuilder1);
     }
    if (productDesc != null && productDesc.length() > 0) {
      MatchQueryBuilder queryBuilder2 = QueryBuilders.matchQuery("productDesc", productDesc);
      builder.must(queryBuilder2);
     }
   }

  // 2.构建查询条件
  NativeSearchQuery query = new NativeSearchQueryBuilder().withQuery(builder).build();

  // 3.查询
  SearchHits<Product> result = template.search(query, Product.class);

  // 4.处理查询结果
  for (SearchHit<Product> productSearchHit : result) {
    Product product = productSearchHit.getContent();
    System.out.println(product);
   }
}
```

#### 分页查询



```java
// 分页查询文档
@Test
public void searchDocumentPage() {
  // 1.确定查询方式
  MatchAllQueryBuilder builder = QueryBuilders.matchAllQuery();


  // 2.构建查询条件
  // 分页条件
  Pageable pageable = PageRequest.of(0, 3);
  NativeSearchQuery query = new NativeSearchQueryBuilder()
     .withQuery(builder)
     .withPageable(pageable)
     .build();


  // 3.查询
  SearchHits<Product> result = template.search(query, Product.class);


  // 4.将查询结果封装为Page对象
  List<Product> content = new ArrayList();
  for (SearchHit<Product> productSearchHit : result) {
    Product product = productSearchHit.getContent();
    content.add(product);
   }
  /**
     * 封装Page对象，参数1：具体数据，参数2：分页条件对象，参数3：总条数
     */
  Page<Product> page = new PageImpl(content, pageable, result.getTotalHits());


  System.out.println(page.getTotalElements());
  System.out.println(page.getTotalPages());
  System.out.println(page.getContent());
}

```

#### 结果排序

```java
@Test
public void searchDocumentSort() {
  // 1.确定查询方式
  MatchAllQueryBuilder builder = QueryBuilders.matchAllQuery();


  // 2.构建查询条件
  // 排序条件
  SortBuilder sortBuilder = SortBuilders.fieldSort("id").order(SortOrder.DESC);
  NativeSearchQuery query = new NativeSearchQueryBuilder()
     .withQuery(builder)
     .withSorts(sortBuilder)
     .build();


  // 3.查询
  SearchHits<Product> result = template.search(query, Product.class);


  // 4.处理查询结果
  for (SearchHit<Product> productSearchHit : result) {
    Product product = productSearchHit.getContent();
    System.out.println(product);
   }
}
```



## 八、ESElasticsearch集群

### 1、概念

在单台ES服务器上，随着一个索引内数据的增多，会产生存储、效率、安全等问题。

1. 假设项目中有一个500G大小的索引，但我们只有几台200G硬盘的服务器，此时是不可能将索引放入其中某一台服务器中的。![image-20221226101157193](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221226101157193.png)

2. 此时我们需要将索引拆分成多份，分别放入不同的服务器中，此时这几台服务器维护了同一个索引，我们称这几台服务器为一个**集群**，其中的每一台服务器为一个**节点**，每一台服务器中的数据称为一个**分片**。

   ![image-20221226101300188](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221226101300188.png)

3. 此时如果某个节点故障，则会造成集群崩溃，所以每个节点的分片往往还会创建**副本**，存放在**其他节点**中，此时一个节点的崩溃就不会影响整个集群的正常运行。

   ![image-20221226101530509](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221226101530509.png)

   

   **节点（node）**：一个节点是集群中的一台服务器，是集群的一部分。它存储数据，参与集群的索引和搜索功能。集群中有一个为主节点，主节点通过ES内部选举产生。

   **集群（cluster）**：一组节点组织在一起称为一个集群，它们共同持有整个的数据，并一起提供索引和搜索功能。

   **分片（shards）**：ES可以把完整的索引分成多个分片，分别存储在不同的节点上。

   **副本（replicas）**：ES可以为每个分片创建副本，提高查询效率，保证在分片数据丢失后的恢复。

   >注：
   >
   >分片的数量只能在索引创建时指定，索引创建后不能再更改分片数量，但可以改变副本的数量。
   >
   >为保证节点发生故障后集群的正常运行，ES不会将某个分片和它的副本存在同一台节点上。

### 2、搭建集群

#### 安装第一个ES节点

1. 安装

   ```shell
   #解压：
   tar -zxvf elasticsearch-7.17.0-linux-x86_64.tar.gz
   
   #重命名：
   mv elasticsearch-7.17.0 myes1
   
   #移动文件夹：
   mv myes1 /usr/local/
   
   #安装ik分词器
   unzip /elasticsearch-analysis-ik-7.17.0.zip -d /usr/local/myes1/plugins/analysis-ik
   
   #安装拼音分词器
   unzip /elasticsearch-analysis-pinyin-7.17.0.zip -d /usr/local/myes1/plugins/analysis-pinyin
   
   #es用户取得该文件夹权限：
   chown -R es:es /usr/local/myes1
   ```

2. 修改配置文件

   ```shell
   #打开节点一配置文件：
   vim /usr/local/myes1/config/elasticsearch.yml
   ```

   配置如下信息：

   ```shell
   #集群名称，保证唯一
   cluster.name: my_elasticsearch
   #节点名称，必须不一样
   node.name: node1
   #可以访问该节点的ip地址
   network.host: 0.0.0.0
   #该节点服务端口号
   http.port: 9200
   #集群间通信端口号
   transport.tcp.port: 9300
   #候选主节点的设备地址
   discovery.seed_hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
   #候选主节点的节点名
   cluster.initial_master_nodes: ["node1","node2","node3"]
   ```

3. 启动

   ```shell
   #切换为es用户：
   su es
   
   #后台启动第一个节点：
   ES_JAVA_OPTS="-Xms512m -Xmx512m" /usr/local/myes1/bin/elasticsearch -d
   ```

#### 安装第二个ES节点

1. 安装

   ```shell
   #解压：
   tar -zxvf elasticsearch-7.17.0-linux-x86_64.tar.gz
   
   #重命名：
   mv elasticsearch-7.17.0 myes2
   
   #移动文件夹：
   mv myes2 /usr/local/
   
   #安装ik分词器
   unzip elasticsearch-analysis-ik-7.17.0.zip -d /usr/local/myes2/plugins/analysis-ik
   
   #安装拼音分词器
   unzip /elasticsearch-analysis-pinyin-7.17.0.zip -d /usr/local/myes2/plugins/analysis-pinyin
   
   #es用户取得该文件夹权限：
   chown -R es:es /usr/local/myes2
   ```

2. 修改配置文件

   ```shell
   #打开节点二配置文件：
   vim /usr/local/myes2/config/elasticsearch.yml
   ```

   配置如下信息：

   ```shell
   #集群名称，保证唯一
   cluster.name: my_elasticsearch
   #节点名称，必须不一样
   node.name: node2
   #可以访问该节点的ip地址
   network.host: 0.0.0.0
   #该节点服务端口号
   http.port: 9201
   #集群间通信端口号
   transport.tcp.port: 9301
   #候选主节点的设备地址
   discovery.seed_hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
   #候选主节点的节点名
   cluster.initial_master_nodes: ["node1","node2","node3"]
   ```

3. 启动

   ```shell
   #切换为es用户：
   su es
   
   #后台启动第二个节点：
   ES_JAVA_OPTS="-Xms512m -Xmx512m" /usr/local/myes2/bin/elasticsearch -d
   ```

#### 安装第三个ES节点

1. 安装

   ```shell
   #解压：
   tar -zxvf elasticsearch-7.17.0-linux-x86_64.tar.gz
   
   #重命名：
   mv elasticsearch-7.17.0 myes3
   
   #移动文件夹：
   mv myes3 /usr/local/
   
   #安装ik分词器
   unzip elasticsearch-analysis-ik-7.17.0.zip -d /usr/local/myes3/plugins/analysis-ik
   
   #安装拼音分词器
   unzip /elasticsearch-analysis-pinyin-7.17.0.zip -d /usr/local/myes3/plugins/analysis-pinyin
   
   #es用户取得该文件夹权限：
   chown -R es:es /usr/local/myes3
   ```

2. 修改配置文件

   ```shell
   #打开节点一配置文件：
   vim /usr/local/myes3/config/elasticsearch.yml
   ```

   配置如下信息：

   ```shell
   #集群名称，保证唯一
   cluster.name: my_elasticsearch
   #节点名称，必须不一样
   node.name: node3
   #可以访问该节点的ip地址
   network.host: 0.0.0.0
   #该节点服务端口号
   http.port: 9202
   #集群间通信端口号
   transport.tcp.port: 9302
   #候选主节点的设备地址
   discovery.seed_hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
   #候选主节点的节点名
   cluster.initial_master_nodes: ["node1","node2","node3"]
   ```

3. 启动

   ```shell
   #切换为es用户：
   su es
   
   #后台启动第三个节点：
   ES_JAVA_OPTS="-Xms512m -Xmx512m" /usr/local/myes3/bin/elasticsearch -d
   ```

#### 测试集群

访问`http://虚拟机IP:9200/_cat/nodes`查看是否集群搭建成功。

#### kibana连接es集群

1. 在kibana中访问集群

   ```shell
   # 打开kibana配置文件
   vim /usr/local/kibana-7.17.0-linux-x86_64/config/kibana.yml
   ```

   添加如下配置

   ```shell
   # 该集群的所有节点
   elasticsearch.hosts: ["http://虚拟机IP:9200","http://虚拟机IP:9201","http://虚拟机IP:9202"]
   ```

2. 启动kibana

   ```shell
   #切换为es用户：
   su es
   
   #启动kibana：
   /usr/local/kibana-7.17.0-linux-x86_64/bin/kibana
   ```

3. 访问kibana：`http://虚拟机IP:5601`

### 3、测试集群状态

#### 在集群中创建索引

```json
PUT /product1
{
 "settings": {
  "number_of_shards": 5, // 分片数
  "number_of_replicas": 1 // 每个分片的副本数
  },
 "mappings": {
  "properties": {
   "id": {
    "type": "integer",
    "store": true,
    "index": true
    },
   "productName": {
    "type": "text",
    "store": true,
    "index": true
    },
   "productDesc": {
    "type": "text",
    "store": true,
    "index": true
    }
   }
  }
}
```

#### 查看集群状态

```json
# 查看集群健康状态
GET /_cat/health?v
# 查看索引状态
GET /_cat/indices?v
# 查看分片状态
GET /_cat/shards?v 
```

### 4、故障应对&水平扩容

1. 关闭一个节点，可以发现ES集群可以自动进行故障应对。

2. 重新打开该节点，可以发现ES集群可以自动进行水平扩容。

3. 分片数不能改变，但是可以改变每个分片的副本数：

   ```json
   PUT /索引/_settings
   {
     "number_of_replicas": 副本数
   }
   ```

## 九、Elasticsearch优化

### 1、内存设置

ES默认占用内存是4GB，我们可以修改config/jvm.option设置ES的堆内存大小，Xms表示堆内存的初始大小，Xmx表示可分配的最大内存。

- Xmx和Xms的**大小设置为相同**的，可以**减轻伸缩堆大小带来的压力**。
- Xmx和Xms**不要超过物理内存的50%**，因为ES内部的Lucene也要占据一部分物理内存。
- Xmx和Xms**不要超过32GB**，由于Java语言的特性，**堆内存超过32G会浪费大量系统资源**，所以在内存足够的情况下，最终我们都会采用设置为31G：

```shell
-Xms 31g
-Xmx 31g
```

例如：在一台128GB内存的机器中，我们可以创建两个节点，每个节点分配31GB内存。

### 2、磁盘选择

ES的优化即**通过调整参数使得读写性能更快**

磁盘通常是服务器的瓶颈。Elasticsearch重度使用磁盘，磁盘的效率越高，Elasticsearch的执行效率就越高。这里有一些优化磁盘的技巧：

- 使用SSD（固态硬盘），它比机械磁盘优秀多了。
- 使用RAID0模式（将连续的数据分散到多个硬盘存储，这样可以并行进行IO操作）,代价是一块硬盘发生故障就会引发系统故障。
- 不要使用远程挂载的存储。

### 3、分片策略

分片和副本数并不是越多越好。**每个分片的底层都是一个Lucene索引**，**会消耗一定的系统资源**。且搜索请求需要命中索引中的所有分片，**分片数过多会降低搜索性能**。索引的分片数需要架构师和技术人员对业务的增长有预先的判断，一般来说我们遵循以下原则：

- 每个分片占用的硬盘容量**不超过ES的最大JVM的堆空间设置(一般设置不超过32G）**。比如：如果索引的总容量在500G左右，那分片数量在16个左右即可。

- **分片数一般不超过节点数的3倍**。比如：如果集群内有10个节点，则分片数不超过30个。

- **推迟分片分配**：**节点中断后集群会重新分配分片**。**但默认集群会等待一分钟**来查看节点是否重新加入。我们可以设置等待的时长，减少重新分配的次数：

  ```json
  PUT /索引/_settings
  {
    "settings":{
      "index.unassianed.node_left.delayed_timeout":"5m"
     }
  }
  
  ```

- **减少副本数量**：进行写入操作时，需要把写入的数据都同步到副本，副本越多写入的效率就越慢。我们**进行大批量进行写入操作时可以先设置副本数为0**，**写入完成后再修改回正常的状态**。

## 十、Elasticsearch案例

