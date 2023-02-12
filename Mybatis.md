## 什么是框架？

<img src="./mybatis_imgs\image-20211018151723386.png" alt="image-20211018151723386" style="zoom:50%;" />

框架即一个半成品软件。开发者从头开发一个软件需要花费大量精力，于是有一些项目组开发出半成品软件，开发者在这些软件的基础上进行开发，这样的软件就称之为框架。

如果将开发完成的软件比作是一套已经装修完毕的新房，框架就好比是一套已经修建好的毛坯房。用户直接购买毛坯房，保证建筑质量和户型合理的同时可以进行风格的自由装修。

> 使用框架开发的好处：
>
> 1. 省去大量的代码编写、减少开发时间、降低开发难度。
> 2. 限制程序员必须使用框架规范开发，增强代码的规范性，降低程序员之间沟通及日后维护的成本。
> 3. 将程序员的注意力从技术中抽离出来，更集中在业务层面。

使用框架就好比和世界上最优秀的软件工程师共同完成一个项目，并且他们完成的还是基础、全局的工作。

## 什么是ORM框架？

![image-20211018164759832](./mybatis_imgs\image-20211018164759832.png)

ORM（Object Relationl Mapping），对象关系映射，即在数据库和对象之间作映射处理。

之前我们使用JDBC操作数据库，必须手动进行数据库和对象间的数据转换。

```java
// 新增方法，将对象转为sql语句字段
public void AddUser(User user) throws Exception {
  Class.forName("com.mysql.jdbc.Driver");
  Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8","root", "root");
  String sql = "INSERT INTO user values (null,?,?,?,?)";


  PreparedStatement preparedStatement = connection.prepareStatement(sql);
  preparedStatement.setString(1,user.getName());
  preparedStatement.setInt(2,user.getAge());
  preparedStatement.setString(3,user.getAddress());
  preparedStatement.setString(4,user.getSex());
  preparedStatement.executeUpdate();
  // 省略资源关闭...
}




// 查询方法，将数据库结果集转为对象
public List<User> findAllUser() throws Exception {
  Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "root");
  PreparedStatement preparedStatement = connection.prepareStatement("select * from user");
  ResultSet resultSet = preparedStatement.executeQuery();


  //遍历查询结果集
  List<User> users = new ArrayList<>();
  while(resultSet.next()){
    // 拿到每一列数据
    int id = resultSet.getInt("id");
    String name = resultSet.getString("name");
    int age = resultSet.getInt("age");
    String address = resultSet.getString("address");
    String sex = resultSet.getString("sex");
    // 将数据封装到对象中
    User user = new User();
    user.setId(id);
    user.setName(name);
    user.setAge(age);
    user.setAddress(address);
    user.setSex(sex);
    users.add(user);
   }
  // 省略资源关闭...
  return users;
}
```

这段代码中，数据库数据与对象数据的转换代码繁琐、无技术含量。而使用ORM框架代替JDBC后，框架可以帮助程序员自动进行转换，只要像平时一样操作对象，ORM框架就会根据映射完成对数据库的操作，极大的增强了开发效率。

## 什么是MyBatis?

<img src="./mybatis_imgs/image-20211018174135045.png?v=1.0.0" alt="image-20211018174135045" style="zoom:67%;" />

MyBatis是一个半自动的ORM框架，其本质是对JDBC的封装。使用MyBatis不需要写JDBC代码，但需要程序员编写SQL语句。之前是apache的一个开源项目iBatis，2010年改名为MyBatis。

> 补充：
>
> Hibernate也是一款持久层ORM框架，多年前的市场占有率很高，但近年来市场占有率越来越低。
>
> MyBatis与Hibernate的比较：
>
> - MyBatis是一个半自动的ORM框架，需要手写SQL语句。
> - Hibernate是一个全自动的ORM框架，不需要手写SQL语句。
> - 使用MyBatis的开发量要大于Hibernate。
>
> 为什么Hibernate市场占有率越来越低：
>
> - 对于新手学习Hibernate时间成本比MyBatis大很多，MyBatis上手很快。
> - Hibernate不需要写SQL语句是因为框架来生成SQL语句。对于复杂查询，开发者很难控制生成的SQL语句，这就导致SQL调优很难进行。
> - 之前的项目功能简单，数据量小，所以使用Hibernate可以快速完成开发。而近年来项目的数据量越来越大，而互联网项目对查询速度要求也很高，这就要求我们一定要精细化的调整SQL语句。此时灵活性更强，手动编写SQL语句的MyBatis慢慢代替了Hibernate使用。
> - 在高并发、大数据、高性能、高响应的互联网项目中，MyBatis是首选的持久框架。而对于对性能要求不高的比如内部管理系统等可以使用Hibernate。

## MyBatis入门

![image-20211018174736011](./mybatis_imgs/image-20211018174736011.png?v=1.0.0)

### 环境搭建 *

1. 将SQL文件导入数据库

2. 创建maven工程，引入依赖

   ```xml
   <dependencies>
     <!--  mybatis  -->
     <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.7</version>
     </dependency>
     <!--  mysql驱动包  -->
     <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.26</version>
     </dependency>
   
   
     <!--  junit  -->
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.10</version>
     </dependency>
     <!--  log4j  -->
     <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.12</version>
     </dependency>
   </dependencies>
   ```

3. **创建mybatis核心配置文件SqlMapConfig.xml**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration
       PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
     <!--  配置环境  -->
     <environments default="mysql">
       <environment id="mysql">
         <!--  事务类型  -->
         <transactionManager type="JDBC"></transactionManager>
         <!--  数据源  -->
         <dataSource type="POOLED">
           <property name="driver" value="com.mysql.jdbc.Driver"/>
           <property name="url" value="jdbc:mysql:///mybatis"/>
           <property name="username" value="root"/>
           <property name="password" value="root"/>
         </dataSource>
       </environment>
     </environments>
   </configuration>
   ```

4. 将log4j.properties文件放入resources中，让控制台打印SQL语句。

5. 创建实体类

   ```java
   public class User {
     private int id;
     private String username;
     private String sex;
     private String address;
       // 省略getter/setter/构造方法/toString方法
   }
   ```

### 创建持久层接口和映射文件

1. 在java目录创建持久层接口

   ```java
   public interface UserMapper {
     List<User> findAll();
   }
   ```

2. 在resource目录创建映射文件

   ```xml-dtd
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.itbaizhan.mapper.UserMapper">
     <select id="findAll" resultType="com.itbaizhan.pojo.User">
        select * from user
     </select>
   </mapper>
   ```

3. 将映射文件配置到mybatis核心配置文件中

   ```xml-dtd
   <!--  注册映射文件  -->
   <mappers>
     <mapper resource="com/itbaizhan/mapper/UserMapper.xml">     </mapper>
   </mappers>
   ```

> 映射文件注意事项：
>
> - 映射文件要和接口名称相同。
>
> - 映射文件要和接口的目录结构相同。
>
>   <img src="./mybatis_imgs/屏幕截图 2023-01-01 143500.png" alt="image-20211101170541119" style="zoom:50%;" />
>
> - 映射文件中namespace属性要写接口的全名。
>
> - 映射文件中标签的id属性是接口方法的方法名。
>
> - 映射文件中标签的resultType属性是接口方法的返回值类型。
>
> - 映射文件中标签的parameterType属性是接口方法的参数类型。
>
> - 映射文件中resultType、parameterType属性要写全类名，如果是集合类型，则写其泛型的全类名。

### 测试持久层接口方法 *

```java
@Test
public void testFindAll() throws Exception {
  // （1）读取核心配置文件
  InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
  // （2）创建SqlSessionFactoryBuilder对象
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  // （3）SqlSessionFactoryBuilder对象获取SqlSessionFactory对象
  SqlSessionFactory factory = builder.build(is);
  // （4）SqlSessionFactory对象获取SqlSession对象
  SqlSession session = factory.openSession();
  // （5）SqlSession对象获取代理对象
  UserMapper userMapper = session.getMapper(UserMapper.class);
  // （6）代理对象执行方法
  List<User> all = userMapper.findAll();
  all.forEach(System.out::println);

  // （7）释放资源
  session.close();
  is.close();
}
```

## MyBatis核心对象及工作流程

![image-20211020104205635](./mybatis_imgs/image-20211020104205635.png?v=1.0.0)

### MyBatis核心对象

- SqlSessionFactoryBuilder

  SqlSession工厂构建者对象，使用构造者模式创建SqlSession工厂对象。

- SqlSessionFactory

  SqlSession工厂，使用工厂模式创建SqlSession对象。

- SqlSession

  该对象可以操作数据库，也可以使用动态代理模式创建持久层接口的代理对象操作数据库。

- Mapper

  持久层接口的代理对象，他具体实现了持久层接口，用来操作数据库。

### MyBatis工作流程

1. 创建SqlSessionFactoryBuilder对象
2. SqlSessionFactoryBuilder对象构建了SqlSessionFactory对象：构造者模式
3. SqlSessionFactory对象生产了SqlSession对象：工厂模式
4. SqlSession对象创建了持久层接口的代理对象：动态代理模式
5. 代理对象操作数据库



## 使用SqlSession操作数据库 *

除了代理对象能够操作数据库，SqlSession也能操作数据库。只是这种方式在开发中使用的较少，接下来我们使用SqlSession操作数据库：

```java
@Test
public void testFindAll2() throws Exception {
  // （1）读取核心配置文件
  InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
  // （2）创建SqlSessionFactoryBuilder对象
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  // （3）SqlSessionFactoryBuilder对象获取SqlSessionFactory对象
  SqlSessionFactory factory = builder.build(is);
  // （4）SqlSessionFactory对象获取SqlSession对象
  SqlSession session = factory.openSession();
  // （5）SqlSession直接操作数据库
  List<User> users = session.selectList("com.itbaizhan.mapper.UserMapper.findAll");
  users.forEach(System.out::println);
  // （6）关闭资源
  session.close();
  is.close();
}
```

## Mapper动态代理原理

<img src="./mybatis_imgs/image-20211019164821646.png?v=1.0.0" alt="image-20211019164821646" style="zoom:50%;" />

接下来我们通过源码，了解MyBatis的Mapper对象究竟是怎么生成的，他又是如何代理接口的方法。

### 获取代理对象

点开测试类的`getMapper`方法，查看该方法最终调用了什么方法。

<img src="./mybatis_imgs/image-20211019162901115.png?v=1.0.0" alt="image-20211019162901115" style="zoom:80%;" />

当看到`Proxy.newProxyInstance`时，可以确定`getMapper`方法最终调用的是JDK动态代理方法，且使用MapperProxy类定义代理方式

### 查看代理方式

点开MapperProxy类，查看invoke方法，查看代理对象是如何工作的。

![image-20211019163829272](./mybatis_imgs/image-20211019163829272.png?v=1.0.0)

可以看到，MapperProxy调用了MapperMethod的execute方法定义了代理方式，且底层调用的是SqlSession的方法，根据映射文件标签不同调用不同的SqlSession方法。

> 结论：
>
> - SqlSession的getMapper方法，最终是调用的是JDK动态代理方法，生成一个代理对象，类型就是传入的接口类型。
> - MapperProxy对象通过调用MapperMethod的execute方法定义了代理方式，该方法的底层调用的是SqlSession的方法。

## MyBatis增删改查 ***

<img src="./mybatis_imgs/image-20211020104338896.png?v=1.0.0" alt="image-20211020104338896" style="zoom:67%;" />

### 注意事项

> 第2步后，还需要在核心配置文件 sqlmapconfig.xml中添加映射文件！！！
>
> 参考上的 **Mybatis入门 》创建持久层接口和映射文件**章节

### 新增用户

1. 持久层接口添加方法

   ```
   void add(User user);
   ```

2. 映射文件添加标签

   ```xml-dtd
   <insert id="add" parameterType="com.itbaizhan.pojo.User">
      insert into user(username,sex,address) values(#{username},#{sex},#{address})
   </insert>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testAdd() throws Exception {
     InputStream is= Resources.getResourceAsStream("SqlMapConfig.xml");
     SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
     SqlSessionFactory factory = builder.build(is);
     SqlSession session = factory.openSession();
     UserMapper userMapper = session.getMapper(UserMapper.class);
     User user = new User("程序员", "男", "上海");
     userMapper.add(user);
     // 提交事务
     session.commit();
     session.close();
     is.close();
   }
   ```

   > 注意：
   >
   > 1. 当接口方法的参数类型为POJO类型时，SQL语句中绑定参数时使用`#{POJO的属性名}`即可。
   > 2. MyBatis事务默认手动提交，所以在执行完增删改方法后，需要手动调用SqlSession对象的事务提交方法，否则数据库将不发生改变。


### 优化测试类

我们发现MyBatis的测试方法在操作数据库前都需要获取代理对象，操作数据库后都需要释放资源，可以利用Junit的前置后置方法，优化测试类代码。

```java
InputStream is = null;
SqlSession session = null;
UserMapper userMapper = null;


@Before
public void before() throws IOException {
  // （1）读取核心配置文件
  is = Resources.getResourceAsStream("SqlMapConfig.xml");
  // （2）创建SqlSessionFactoryBuilder对象
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  // （3）SqlSessionFactoryBuilder对象获取SqlSessionFactory对象
  SqlSessionFactory factory = builder.build(is);
  // （4）SqlSessionFactory对象获取SqlSession对象
  session = factory.openSession();
  // （5）获取代理对象
  userMapper = session.getMapper(UserMapper.class);
}


@After
public void after() throws IOException {
  // 释放资源
  session.close();
  is.close();
}
```

这样Junit就会自动执行获取代理对象和释放资源的方法。

### 修改用户

1. 持久层接口添加方法

   ```java
   void update(User user);
   ```

2. 编写测试方法

   ```xml
   <update id="update" parameterType="com.itbaizhan.pojo.User">
      update user
      set username = #{username},
      sex    = #{sex},
      address=#{address}
      where id = #{id}
   </update>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testUpdate(){
     User user = new User(8,"程序员1","女","深圳");
     userMapper.update(user);
     session.commit();
   }
   ```

### 删除用户

1. 持久层接口添加方法

   ```java
   void delete(int userId);
   ```

2. 映射文件添加标签

   ```xml
   <delete id="delete" parameterType="int">
      delete from user where id = #{id}
   </delete>
   ```

   > 注：当方法的参数类型是简单数据类型时，#{}中可以写任意名称
   >
   > - 简单数据类型：基本数据类型、字符串等

3. 编写测试方法

   ```java
   @Test
   public void testDelete(){
     userMapper.delete(8);
     session.commit();
   }
   ```

### 根据ID查询用户

1. 持久层接口添加方法

   ```java
   User findById(int userId);
   ```
   
2. 映射文件添加标签

   ```xml
<select id="findById" parameterType="int" resultType="com.itbaizhan.pojo.User">
      select * from user where id = #{userId}
</select>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testFindById(){
     User user = userMapper.findById(1);
     System.out.println(user);
   }
   ```

## 模糊查询 ***

<img src="./mybatis_imgs/image-20211020142812190.png" alt="image-20211020142812190" style="zoom:67%;" />

#### 使用#定义参数

1. 持久层接口添加方法

   ```java
   List<User> findByNameLike(String username);
   ```

2. 映射文件添加标签

   ```xml
   <select id="findByNameLike" parameterType="string" resultType="com.itbaizhan.user.User">
      select * from user where username like #{name}
   </select>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testFindByNameLike(){
     List<User> users = userMapper.findByNameLike("%王%");
     for (User user:users){
       System.out.println(user);
      }
   }
   ```

> 我们看到在映射文件中，parameterType的值为`string`而没有写`java.lang.String`，这是为什么呢？
>
> - 参数/返回值类型为基本数据类型/包装类/String等类型时，我们可以写全类名，也可以写别名。

| 数据类型   | 别名               |
| ---------- | ------------------ |
| byte       | _byte              |
| long       | _long              |
| short      | _short             |
| int        | _int               |
| int        | _integer           |
| double     | _double            |
| float      | _float             |
| boolean    | _boolean           |
| String     | string             |
| Byte       | byte               |
| Long       | long               |
| Short      | short              |
| Integer    | int/integer        |
| Double     | double             |
| Float      | float              |
| Boolean    | boolean            |
| Date       | date               |
| BigDecimal | decimal/bigdecimal |
| Object     | object             |
| Map        | map                |
| HashMap    | hashmap            |
| List       | list               |
| ArrayList  | arraylist          |
| Collection | collection         |
| Iterator   | iterator           |

#### 使用$定义参数 ***

模糊查询如果不想在调用方法时参数加%，可以使用拼接参数的方式设置Sql：

```xml
<select id="findByUsernameLike" parameterType="string" resultType="com.itbaizhan.pojo.User">
   select * from user where username like '%${value}%'
</select>
```

测试方法写法如下：

```java
@Test
public void testFindByNameLike(){
  List<User> users = userMapper.findByUsernameLike("尚学堂");
  users.forEach(System.out::println);
}
```

> \#和$的区别：
>
> 1. \#表示sql模板的占位符，$表示将字符串拼接到sql模板中。
> 2. \#可以防止sql注入，一般能用#就不用$。
> 3. ${}内部的参数名必须写value。

#### 使用`<bind>`定义参数

如果使用`#`还不想在调用方法的参数中添加`%`，可以使用`<bind>`，`<bind>`允许我们在 Sql语句以外创建一个变量，并可以将其绑定到当前的Sql语句中。用法如下：

```xml
<select id="findByUsernameLike" parameterType="string" resultType="com.itbaizhan.pojo.User">
  <bind name="likeName" value="'%'+username+'%'"/>
   select * from user where username like #{likeName}
</select>
```

测试方法写法如下：

```java
@Test
public void testFindByNameLike(){
  List<User> users = userMapper.findByUsernameLike("尚学堂");
  users.forEach(System.out::println);
}
```

### 分页查询 ***

<img src="./mybatis_imgs/image-20211020143324365.png?v=1.0.0" alt="image-20211020143324365" style="zoom:67%;" />

分页查询时，Sql语句使用limit关键字，需要传入开始索引和每页条数两个参数。MyBatis的多参数处理有以下方式：

#### 顺序传参 

Sql中的参数使用arg0，arg1...或param1，param2...表示参数的顺序。此方法可读性较低，在开发中不建议使用。

1. 持久层接口方法

   ```java
   /**
      * 分页查询
      * @param startIndex 开始索引
      * @param pageSize 每页条数
      * @return
      */
   List<User> findPage(int startIndex,int pageSize);
   ```

2. 映射文件

   ```xml
   <select id="findPage" resultType="com.itbaizhan.mapper.User">
      select * from user limit #{arg0},#{arg1}
   </select>
   
   
   <select id="findPage" resultType="com.itbaizhan.mapper.User">
      select * from user limit #{param1},#{param2}
   </select>
   ```

3. 测试类

   ```java
   @Test
   public void testFindPage(){
     List<User> users = userMapper.findPage(0,3);
     users.forEach(System.out::println);
   }
   ```

#### @Param传参

在接口方法的参数列表中通过@Param定义参数名称，在Sql语句中通过注解中所定义的参数名称指定参数位置。此方式参数比较直观的，推荐使用。

1. 持久层接口方法

   ```java
   List<User> findPage1(@Param("startIndex") int startIndex, @Param("pageSize")int pageSize);
   ```

2. 映射文件

   ```xml
   <select id="findPage1" resultType="com.itbaizhan.mapper.User">
     select * from user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试类

   ```java
   @Test
   public void testFindPage1(){
     List<User> users = userMapper.findPage1(3,3);
     users.forEach(System.out::println);
   }
   ```

#### POJO传参  ***

自定义POJO类，该类的属性就是要传递的参数，在SQL语句中绑定参数时使用POJO的属性名作为参数名即可。此方式推荐使用。

1. 自定义POJO

   ```java
   public class PageQuery {
     private int startIndex;
     private int pageSize;
       // 省略getter/setter/构造方法
   }
   ```

2. 持久层接口方法

   ```java
   List<User> findPage2(PageQuery pageQuery);
   ```

3. 映射文件

   ```xml
   <select id="findPage2" resultType="com.itbaizhan.pojo.User" parameterType="com.itbaizhan.pojo.PageQuery">
      select * from user limit #{startIndex},#{pageSize}
   </select>
   ```

4. 测试类

   ```java
   @Test
   public void testFindPage2(){
     PageQuery pageQuery = new PageQuery(3, 3);
     List<User> users = userMapper.findPage2(pageQuery);
     users.forEach(System.out::println);
   }
   ```

#### Map传参

如果不想自定义POJO，可以使用Map作为传递参数的载体，在SQL语句中绑定参数时使用Map的Key作为参数名即可。此方法推荐使用。

1. 持久层接口方法

   ```java
   List<User> findPage3(Map<String,Object> params);
   ```

2. 映射文件

   ```xml
   <select id="findPage3" resultType="com.itbaizhan.pojo.User" parameterType="map">
      select * from user limit #{startIndex},#{pageSize}
   </select>
   ```

3. 测试类

   ```java
   @Test
   public void testFindPage3(){
     Map<String,Object> params = new HashMap();
     params.put("startIndex",0);
     params.put("pageSize",4);
     List<User> users = userMapper.findPage3(params);
     users.forEach(System.out::println);
   } @Test
   public void testFindPage3(){
     Map<String,Object> params = new HashMap();
     params.put("startIndex",0);
     params.put("pageSize",4);
     List<User> users = userMapper.findPage3(params);
     users.forEach(System.out::println);
   }
   ```

### 聚合查询_查询用户总数

1. 持久层接口方法

   ```java
   int findCount();
   ```

2. 映射文件

   ```xml
   <select id="findCount" resultType="int">
      select count(id) from user
   </select>
   ```

3. 测试类

   ```java
   @Test
   public void testFindCount(){
     System.out.println(userMapper.findCount());
   }
   ```

### 主键回填

有时我们需要获取新插入数据的主键值。如果数据库中主键是自增的，这时我们就需要使用MyBatis的主键回填功能。

1. 持久层接口方法

   ```java
   void add(User user);
   ```
   
2. 映射文件

   ```xml
   <insert id="add" parameterType="com.itbaizhan.user.User">
     <!-- keyProperty:主键属性名，keyColumn:主键列名，resultType:主键类型，order:执行时机 -->
     <selectKey keyProperty="id" keyColumn="id" resultType="int" order="AFTER">
        SELECT LAST_INSERT_ID();
     </selectKey>
      insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
   </insert>
   ```
   
   > SELECT LAST_INSERT_ID()：查询刚刚插入的记录的主键值，只适用于自增主键，且必须和insert语句一起执行。
   
3. 测试类

   ```java
   @Test
   public void testAdd(){
     User user = new User("尚学堂", new Date(), "男", "北京");
     userMapper.add(user);
     session.commit();
     System.out.println(user.getId());
   }
   ```

## MyBatis配置文件  各种标签 ***

<img src="./mybatis_imgs/image-20211020160252116.png?v=1.0.0" alt="image-20211020160252116" style="zoom: 50%;" />

### 标签结构

MyBatis配置文件结构：

```yaml
-configuration
    -properties（属性）
        -property
    -settings（全局配置参数）
        -setting
    -plugins（插件）
        -plugin
    -typeAliases（别名）
        -typeAliase
        -package
    -environments（环境）
        -environment
            -transactionManager（事务管理）
            -dataSource（数据源）
    -mappers（映射器）
        -mapper
        -package
```

### properties 

#### `<properties>`

属性值定义。properties标签中可以定义属性值，**也可以引入外部配置文件**。无论是内部定义还是外部引入，都**可以使用${name}获取值**。

例如：我们可以**将数据源配置写到外部**的db.properties中，再**使用properties标签引入外部配置文件**，这样可以做到动态配置数据源。

1. 编写db.properties

   ```xml
   jdbc.driver=com.mysql.jdbc.Driver
   jdbc.url=jdbc:mysql://localhost:3306/mybatis
   jdbc.username=root
   jdbc.password=root
   ```

2. 在配置文件中引入db.properties

   ```xml
   <properties resource="db.properties"></properties>
   <environments default="mysql">
     <environment id="mysql">
       <transactionManager type="JDBC"></transactionManager>
       <dataSource type="POOLED">
         <property name="driver" value="${jdbc.driver}"/>
         <property name="url" value="${jdbc.url}"/>
         <property name="username" value="${jdbc.username}"/>
         <property name="password" value="${jdbc.password}"/>
       </dataSource>
     </environment>
   </environments>
   ```

当然我们也可以将数据源数据通过`<properties>`配置到MyBatis配置文件内，但这样做没什么意义。

```xml
<properties>
  <property name="jdbc.driver" value="com.mysql.jdbc.Driver"></property>
  <property name="jdbc.url" value="jdbc:mysql://localhost:3306/mybatis"></property>
  <property name="jdbc.username" value="root"></property>
  <property name="jdbc.password" value="root"></property>
</properties>


<environments default="mysql">
  <environment id="mysql">
    <transactionManager type="JDBC"></transactionManager>
    <dataSource type="POOLED">
      <property name="driver" value="${jdbc.driver}"/>
      <property name="url" value="${jdbc.url}"/>
      <property name="username" value="${jdbc.username}"/>
      <property name="password" value="${jdbc.password}"/>
    </dataSource>
  </environment>
</environments>
```

### settings

#### `<settings>`

`<settings>`是配置MyBatis运行时的一些行为的，例如缓存、延迟加载、命名规则等一系列控制性参数。后期我们会使用该标签配置缓存和延迟加载等。

### plugins

#### `<plugins>`

`<plugins>`是配置MyBatis插件的。插件可以增强MyBatis功能，比如进行sql增强，打印日志，异常处理等。后期我们会使用该标签配置分页插件。

### typeAliases

#### `<typeAliases>`

![f70fa76f3383dc83eaabef6a53d7eb93](./mybatis_imgs/f70fa76f3383dc83eaabef6a53d7eb93.gif)

MyBatis对常用类有默认别名支持，比如java.lang.Stirng的别名为string。除此之外，我们也可以使用`<typeAliases>`设置自定义别名。

#### 为一个类配置别名

```xml
<typeAliases>
    <typeAlias type="全类名" alias="别名"></typeAlias>
</typeAliases>
```

此时我们即可在映射文件中使用自定义别名，如：

1. 配置文件：

   ```xml
   <typeAliases>
       <typeAlias type="com.itbaizhan.pojo.User" alias="User">     </typeAlias>
   </typeAliases>
   ```

2. 映射文件：

   ```xml
   <select id="findAll" resultType="User">
      select * from user
   </select>
   ```

#### 为一个所有包下的所有类配置别名

```xml
<typeAliases>
  <package name="包名"></package>
</typeAliases>
```

此时该包下的所有类都有了别名，别名省略包名，和类名相同。如：

1. 配置文件：

   ```xml
   <typeAliases>
     <package name="com.itbaizhan.pojo"></package>
   </typeAliases>
   ```

2. 映射文件：

   ```xml
   <select id="findPage2" resultType="User" parameterType="PageQuery">
      select * from user limit #{startIndex},#{pageSize}
   </select>
   ```

### environments

#### `<environments>`

`<environments>`可以为MyBatis配置数据环境。

#### 事务管理

`session.commit` 对事物进行提交操作

```xml
<environments default="mysql">
  <environment id="mysql">
    <!-- JDBC：使用JDBC的提交和回滚 MANAGED：不做事务处理-->
    <transactionManager type="JDBC"></transactionManager>
  </environment>
</environments>
```

#### 连接池

```xml
<environments default="mysql">
  <environment id="mysql">
    <transactionManager type="JDBC"></transactionManager>
    <!-- 连接池设置 -->
    <dataSource type="POOLED">
      <!-- 数据源设置... -->
    </dataSource>
  </environment>
</environments>
```

> dataSource的type属性：
>
> - POOLED：使用连接池管理连接，使用MyBatis自带的连接池。
> - UNPOOLED：不使用连接池，直接由JDBC连接。
> - JNDI：由JAVAEE服务器管理连接，如果使用Tomcat作为服务器则使用Tomcat自带的连接池管理。

### mappers***

#### `<mappers>`

![image-20211020172055016](./mybatis_imgs/image-20211020172055016.png?v=1.0.0)

`<mappers>`用于注册映射文件或持久层接口，只有注册的映射文件才能使用，共有四种方式都可以完成注册：

1. 使用相对路径注册映射文件

   ```xml
   <mappers>
    <mapper resource="com/itbaizhan/mapper/UserMapper.xml"/>
   </mappers>
   ```

2. 使用绝对路径注册映射文件

   ```xml
   <mappers>  
     <mapper url="file:///C:\Users\a\IdeaProjects\mybatiscase\mybatisDemo1\src\main\resources\com\itbaizhan\mapper\UserMapper.xml"/> 
   </mappers>
   ```

3. 注册持久层接口
   ```xml
   <mappers>  
     <mapper class="com.itbaizhan.mapper.UserMapper"/> 
   </mappers>
    <mappers>  
     <mapper class="com.itbaizhan.mapper.UserMapper"/> 
   </mappers>
   ```
   
4. 注册一个包下的所有持久层接口

   ```xml
   <mappers>
     <package name="com.itbaizhan.mapper"/>
   </mappers>
   ```

## MyBatis映射文件 各种标签 ***

MyBatis映射文件中除了`<insert>`、`<delete>`、`<update>`、`<select>`外，还有一些标签可以使用：

### resultMap ***

标签的作用的自定义映射关系。

MyBatis可以将数据库结果集封装到对象中，是因为结果集的列名和对象属性名相同：

![image-20211103144440901](./mybatis_imgs/image-20211103144440901.png?v=1.0.0)

当POJO属性名和数据库列名不一致时，MyBatis无法自动完成映射关系。如：

![image-20211103144728224](./mybatis_imgs/image-20211103144728224.png?v=1.0.0)

#### 此时有两种解决方案：

1. Sql语句的查询字段起与POJO属性相同的别名。

   ```xml
   <select id="findAll" resultType="com.itbaizhan.pojo.Teacher">
      select tid as id,tname as teacherName from teacher;
   </select>
   ```

2. 自定义映射关系

   - 在映射文件中，使用`<resultMap>`自定义映射关系：

   ```xml
   <!-- id:自定义映射名 type：自定义映射的对象类型  -->
   <resultMap id="teacherMapper" type="com.itbaizhan.pojo.Teacher">
     <!-- id定义主键列  property:POJO属性名 column:数据库列名  -->
     <id property="id" column="tid"></id>
     <!-- result定义普通列  property:POJO属性名 column:数据库列名  -->
     <result property="teacherName" column="tname"></result>
   </resultMap>
   ```

   * 在`<select>`标签中，使用`resultMap`属性代替`resultType`属性，使用自定义映射关系。

   ```xml
   <select id="findAll" resultMap="teacherMapper">
      select * from teacher
   </select>
   ```

   



### sql&include



<img src="./mybatis_imgs/image-20211026104210290.png" alt="image-20211026104210290" style="zoom: 33%;" />

`<sql>`用来定义可重用的Sql片段，通过`<include>`引入该片段。如：Sql语句的查询字段起与POJO属性相同的别名，该Sql片段就可以重用。

```xml
<sql id="selectAllField">
   select tid as id,tname as teacherName
</sql>


<select id="findAll" resultType="com.itbaizhan.pojo.Teacher">
  <include refid="selectAllField"></include>
   from teacher;
</select>


<select id="findById" resultType="com.itbaizhan.pojo.Teacher">
  <include refid="selectAllField"></include>
   from teacher where tid = #{id}
</select>
```

### 特殊字符处理

<img src="./mybatis_imgs/image-20211021101247289.png?v=1.0.0" alt="image-20211021101247289" style="zoom:50%;" />

在Mybatis映射文件中尽量不要使用一些特殊字符，如：`<`，`>`等。

我们可以使用符号的实体来表示：

| 符号 | 实体     |
| ---- | -------- |
| <    | `&lt;`   |
| >    | `&gt;`   |
| &    | `&amp;`  |
| ‘    | `&apos;` |
| “    | `&quot;` |

如：

```xml
<select id="findById2" resultType="com.itbaizhan.pojo.Teacher">
  <include refid="selectAllField"></include>
   from teacher where tid &gt; #{id}
</select>
```

## 动态SQL *	

![image-20211103160027828](./mybatis_imgs/image-20211103160027828.png?v=1.0.0)

一个查询的方法的Sql语句不一定是固定的。比如电商网站的查询商品，用户使用不同条件查询，Sql语句就会添加不同的查询条件。此时就需要在方法中使用动态Sql语句。

### if

`<if>`标签内的Sql片段在满足条件后才会添加，用法为：`<if test="条件">`。例如：根据不同条件查询用户：

1. 持久层接口添加方法

   ```java
   // 用户通用查询
   List<User> findByCondition(User user); 
   
   ```

2. 映射文件添加标签

   ```xml
   <select id="findByCondition" parameterType="com.itbaizhan.pojo.User" resultType="com.itbaizhan.pojo.User">
      select * from user where 1 = 1
     <if test="username != null and username.length() != 0">
        and username like #{username}
     </if>
     <if test="sex != null and sex.length() != 0">
        and sex = #{sex}
     </if>
     <if test="address != null and address.length() != 0">
        and address = #{address}
     </if>
   </select>
   ```
   
3. 编写测试方法

   ```java
   @Test
   public void testFindByCondition(){
     User user = new User();
     List<User> users1 = userMapper2.findByCondition(user);
     //users1.forEach(System.out::println);
   
   
     user.setUsername("%尚学堂%");
     List<User> users2 = userMapper2.findByCondition(user);
     users2.forEach(System.out::println);
   
   
     user.setAddress("北京");
     List<User> users3 = userMapper2.findByCondition(user);
     users3.forEach(System.out::println);
   }
   ```

#### 注意事项

> 注意事项!
>
> 1. if中的条件不能使用&&/||，而应该使用and/or
>
> 2. if中的条件可以直接通过属性名获取参数POJO的属性值，并且该值可以调用方法。
>
> 3. where后为什么要加1=1？
>
>    任意条件都可能拼接到Sql中。如果有多个条件，从第二个条件开始前都需要加And关键字。加上1=1这个永久成立的条件，就不需要考虑后面的条件哪个是第一个条件，后面的条件前都加And关键字即可。

### where & set

#### where

`<where>`可以代替sql中的where 1=1 和第一个and，更符合程序员的开发习惯，使用`<where>`后的映射文件如下：

```xml
<select id="findByCondition" resultType="com.itbaizhan.user.User" parameterType="com.itbaizhan.user.User">
   select * from user
  <where>
    <if test="username != null and username.length() != 0">
       username like #{username}
    </if>
    <if test="sex != null and sex.length() != 0">
       and sex = #{sex}
    </if>
  </where>
</select>
```

#### set

`<set>`标签用在update语句中。借助`<if>`，可以只对有具体值的字段进行更新。`<set>`会自动添加set关键字，并去掉最后一个if语句中多余的逗号。

```xml
<update id="update" parameterType="com.itbaizhan.user.User">
   update user
  <set>
    <if test="username != null and username.length() > 0">
       username = #{username},
    </if>
    <if test="sex != null and sex.length() > 0">
       sex = #{sex},
    </if>
  </set>
  <where>
     id = #{id}
  </where>
</update>
```

### choose、when、otherwise

<img src="./mybatis_imgs/image-20211021115534302.png" alt="image-20211021115534302" style="zoom:80%;" />

这些标签表示多条件分支，类似JAVA中的`switch...case`。`<choose>`类似`switch`，`<when>`类似`case`，`<otherwise>`类似`default`，用法如下：

```xml
<select id="findByCondition" resultType="com.itbaizhan.user.User" parameterType="com.itbaizhan.user.User">
   select * from user
  <where>
    <choose>
      <when test="username.length() &lt; 5">
         username like #{username}
      </when>
      <when test="username.length() &lt; 10">
         username = #{username}
      </when>
      <otherwise>
         id = 1
      </otherwise>
    </choose>
  </where>
</select>
```

>  这段代码的含义为：用户名<5时使用模糊查询，用户名>=5并且<10时使用精确查询，否则查询id为1的用户

### foreach ***

![ceeb653ely1g0r3ywpaajg207s057wlr](.\mybatis_imgs\ceeb653ely1g0r3ywpaajg207s057wlr.gif)

`<foreach>`类似JAVA中的for循环，可以遍历集合或数组。`<foreach>`有如下属性：

- collection：遍历的对象类型
- open：开始的sql语句
- close：结束的sql语句
- separator：遍历每项间的分隔符
- item：表示本次遍历获取的元素，遍历List、Set、数组时表示每项元素，遍历map时表示键值对的值。
- index：遍历List、数组时表示遍历的索引，遍历map时表示键值对的键。

#### 遍历数组

我们使用`<foreach>`遍历数组进行批量删除。

1. 持久层接口添加方法

   ```java
   void deleteBatch(int[] ids);
   ```

2. 映射文件添加标签

   ```xml
   <delete id="deleteBatch" parameterType="int">
      delete from user
     <where>
       <foreach open="id in(" close=")" separator="," collection="array" item="id" >
          #{id}
       </foreach>
     </where>
   </delete>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testDeleteBatch(){
     int[] ids = {9,11};
     userMapper.deleteBatch(ids);
     session.commit();
   }
   ```

#### 遍历Collection

`<foreach>`遍历List和Set的方法是一样的，我们使用`<foreach>`遍历List进行批量添加。

1. 持久层接口添加方法

   ```java
   void insertBatch(List<User> users);
   ```

2. 映射文件添加标签

   ```xml
   <insert id="insertBatch" parameterType="com.itbaizhan.user.User">
      insert into user values
     <foreach collection="list" item="user" separator=",">
       #{user.username},#{user.birthday},#{user.sex},#{user.address})
     </foreach>
   </insert>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testDeleteBatch(){
     int[] ids = {1,2,3};
     userMapper.deleteBatch(ids);
     session.commit();
   }
   ```

#### 遍历Map

我们使用`<foreach>`遍历Map进行多条件查询。

1. 持久层接口添加方法

   ```java
   /**
      * 多条件查询
      * @param map 查询的条件键值对 键：属性名 值：属性值
      * @return
      */
   List<User> findUser(@Param("queryMap") Map<String,Object> map);
   ```

2. 映射文件添加标签

   ```xml
   <select id="findUser" parameterType="map" resultType="com.itbaizhan.pojo.User">
      select * from user
     <where>
       <foreach collection="queryMap" separator="and" index="key" item="value">
          ${key} = #{value}
       </foreach>
     </where>
   </select>
   ```

3. 编写测试方法

   ```java
   @Test
   public void testFindUser(){
     Map<String,Object> queryMap = new HashMap();
     queryMap.put("sex","男");
     queryMap.put("address","北京");
     List<User> users = userMapper2.findUser(queryMap);
     users.forEach(System.out::println);
   }
   ```

## 缓存介绍

![image-20211025161213113](./mybatis_imgs\image-20211025161213113.png)

缓存是内存当中一块存储数据的区域，目的是提高**查询**效率。MyBatis会将查询结果存储在缓存当中，当下次执行**相同**的SQL时不访问数据库，而是直接从缓存中获取结果，从而减少服务器的压力。

- 什么是缓存？

  > 存在于内存中的一块数据。

- 缓存有什么作用？

  > 减少程序和数据库的交互，提高查询效率，降低服务器和数据库的压力。

- 什么样的数据使用缓存？

  > 经常查询但不常改变的，改变后对结果影响不大的数据。

- MyBatis缓存分为哪几类？

  > 一级缓存和二级缓存

- 如何判断两次Sql是相同的？

  > 1. 查询的Sql语句相同
  > 2. 传递的参数值相同
  > 3. 对结果集的要求相同
  > 4. 预编译的模板Id相同



## MyBatis一级缓存

<img src=".\mybatis_imgs\image-20211025164107035.png" alt="image-20211025164107035" style="zoom:67%;" />

- MyBatis一级缓存也叫本地缓存。SqlSession对象中包含一个Executor对象，Executor对象中包含一个PerpetualCache对象，在该对象存放一级缓存数据。
- 由于一级缓存是在SqlSession对象中，所以只有使用同一个SqlSession对象操作数据库时才能共享一级缓存。
- MyBatis的一级缓存是默认开启的，不需要任何的配置。

### 测试一级缓存

```java
@Test
public void testCache1() throws IOException {
  InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(is);
  SqlSession session = factory.openSession();


  // 使用同一个SqlSession查询
  UserMapper mapper1 = session.getMapper(UserMapper.class);
  UserMapper mapper2 = session.getMapper(UserMapper.class);


  User user1 = mapper1.findById(1);
  System.out.println(user1.hashCode());
  System.out.println("-------------------------------------------");
  User user2 = mapper2.findById(1);
  System.out.println(user2.hashCode());
}


@Test
public void testCache2() throws IOException {
  InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(is);
  SqlSession session1 = factory.openSession();
  SqlSession session2 = factory.openSession();


  // 使用不同的SqlSession查询
  UserMapper mapper1 = session1.getMapper(UserMapper.class);
  UserMapper mapper2 = session2.getMapper(UserMapper.class);


  User user1 = mapper1.findById(1);
  System.out.println(user1.hashCode());
  System.out.println("-------------------------------------------");
  User user2 = mapper2.findById(1);
  System.out.println(user2.hashCode());
}

```



## MyBatis清空一级缓存

<img src=".\mybatis_imgs\image-20211025171923767.png" alt="image-20211025171923767" style="zoom: 50%;" />

进行以下操作可以清空MyBatis一级缓存：

1. `SqlSession`调用`close()`：操作后SqlSession对象不可用，该对象的缓存数据也不可用。
2. `SqlSession`调用`clearCache()`/`commit()`：操作会清空一级缓存数据。
3. `SqlSession`调用增删改方法：操作会清空一级缓存数据，因为增删改后数据库发生改变，缓存数据将不准确。

```java
@Test
public void testCache3() throws IOException {
  InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(is);
  SqlSession session = factory.openSession();


  UserMapper mapper1 = session.getMapper(UserMapper.class);
  UserMapper mapper2 = session.getMapper(UserMapper.class);


  User user1 = mapper1.findById(1);
  System.out.println(user1.hashCode());
  //     session.close();
  //     session.clearCache();
  //     session.commit();
  mapper1.delete(2);
  System.out.println("-------------------------------------------");
  User user2 = mapper2.findById(1);
  System.out.println(user2.hashCode());
}
```



## MyBatis二级缓存

![image-20211025183225309](.\mybatis_imgs\image-20211025183225309.png)

- MyBatis二级缓存也叫全局缓存。数据存放在SqlSessionFactory中，只要是同一个工厂对象创建的SqlSession，在进行查询时都能共享数据。一般在项目中只有一个SqlSessionFactory对象，所以二级缓存的数据是全项目共享的。

- MyBatis一级缓存存放的是对象，二级缓存存放的是对象的数据。所以要求二级缓存存放的POJO必须是可序列化的，也就是要实现Serializable接口。

- MyBatis二级缓存默认不开启，手动开启后数据先存放在一级缓存中，只有一级缓存数据清空后，数据才会存到二级缓存中。

  > `SqlSession`调用`clearCache()`无法将数据存到二级缓存中。

### 开启二级缓存

1. POJO类实现Serializable接口。

   ```java
   public class User implements Serializable {
     private int id;
     private String username;
     private String sex;
     private String address;
   }
   ```

2. 在MyBatis配置文件添加如下设置：

   ```xml
   <settings>
     <setting name="cacheEnabled" value="true"/>
   </settings>
   ```

   > 由于cacheEnabled默认值是true，所以该设置可以省略。

3. 在映射文件添加<cache />标签，该映射文件下的所有方法都支持二级缓存。

   > 如果查询到的集合中对象过多，二级缓存只能缓存1024个对象引用。可以通过<cache />标签的size属性修改该数量。
   >
   > ```xml
   > <cache size="2048"/>
   > ```

4. 测试二级缓存

   ```java
   @Test
   public void testCache4() throws IOException {
     InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
     SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
     SqlSessionFactory factory = builder.build(is);
     SqlSession session1 = factory.openSession();
     SqlSession session2 = factory.openSession();
   
     UserMapper mapper1 = session1.getMapper(UserMapper.class);
     UserMapper mapper2 = session2.getMapper(UserMapper.class);
   
     User user1 = mapper1.findById(1);
     System.out.println(user1);
     System.out.println(user1.hashCode());
     // 让一级缓存失效
     session1.commit();
     System.out.println("-------------------------------------------");
   
     User user2 = mapper2.findById(1);
     System.out.println(user2);
     System.out.println(user2.hashCode());
   }
   ```

   
   



## MyBatis关联查询

![image-20211026171307814](.\mybatis_imgs\image-20211026171307814.png)

MyBatis的关联查询分为一对一关联查询和一对多关联查询。

> - 查询对象时，将关联的另一个**对象**查询出来，就是一对一关联查询。
> - 查询对象时，将关联的另一个对象的**集合**查询出来，就是一对多关联查询。

例如有学生类和班级类：

一个学生对应一个班级，也就是学生类中有一个班级属性，这就是一对一关系。

一个班级对应多个学生，也就是班级类中有一个学生集合属性，这就是一对多关系。

实体类设计如下：

```java
public class Student {
  private int sid;
  private String name;
  private int age;
  private String sex;
  private Classes classes;
  // 省略getter/setter/toString
}


public class Classes {
  private int cid;
  private String className;
  private List<Student> studentList;
  // 省略getter/setter/toString
}
```

数据库设计如下：

![image-20211026141751107](.\mybatis_imgs\image-20211026141751107.png)

## MyBatis一对一关联查询

<img src=".\mybatis_imgs\image-20211026171820515.png" alt="image-20211026171820515" style="zoom:50%;" />

查询学生时，将关联的一个班级对象查询出来，就是一对一关联查询。

### 创建持久层接口

```java
public interface StudentMapper {
  List<Student> findAll();
}

```

### 创建映射文件

```xml
<resultMap id="studentMapper" type="com.itbaizhan.pojo.Student">
  <!-- 主键列 -->
  <id property="sid" column="sid"></id>
  <!-- 普通列 -->
  <result property="name" column="name"></result>
  <result property="age" column="age"></result>
  <result property="sex" column="sex"></result>
  <!-- 一对一对象列 property:属性名  column:关联列名 javaType：对象类型-->
  <association property="classes" column="classId" javaType="com.itbaizhan.pojo.Classes">
    <!-- 关联对象主键列 -->
    <id property="cid" column="cid"></id>
    <!-- 关联对象普通列 -->
    <result property="className" column="className"></result>
  </association>
</resultMap>


<!-- 多表查询，级联查询学生和其班级 -->
<select id="findAll" resultMap="studentMapper">
   select * from student left join classes on student.classId = classes.cid;
</select>
```

### 配置文件注册映射文件

```xml
<mappers>
  <package name="com.itbaizhan.mapper"/>
</mappers>
```

### 测试一对一关联查询

```java
InputStream is = null;
SqlSession session = null;


@Before
public void before() throws IOException {
  is = Resources.getResourceAsStream("SqlMapConfig.xml");
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  SqlSessionFactory factory = builder.build(is);
  session = factory.openSession();
}


@After
public void after() throws IOException {
  session.close();
  is.close();
}


@Test
public void testFindAllStudent(){
  StudentMapper studentMapper = session.getMapper(StudentMapper.class);
  List<Student> all = studentMapper.findAll();
  all.forEach(System.out::println);
}
```

## MyBatis一对多关联查询

![image-20211026171956904](.\mybatis_imgs\image-20211026171956904.png)

查询班级时，将关联的学生集合查询出来，就是一对多关联查询。

### 创建持久层接口

```java
public interface ClassesMapper {
  List<Classes> findAll();
}
```

### 创建映射文件 ***

```xml
<resultMap id="classesMapper" type="com.itbaizhan.pojo.Classes">
  <id property="cid" column="cid"></id>
  <result property="className" column="className"></result>
    
    
    
  <!-- 集合列  property：属性名  column：关联列名 ofType：集合的泛型 -->
  <collection property="studentList" column="classId" ofType="com.itbaizhan.pojo.Student">
    <id property="sid" column="sid"></id>
    <result property="name" column="name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
  </collection>
</resultMap>


<!-- 多表查询，级联查询班级和它的学生 -->
<select id="findAll" resultMap="classesMapper">
   select * from classes left join student  on classes.cid = student.classId;
</select>
```

### 测试一对多关联查询

```java
@Test
public void testFindAllClasses() {
  ClassesMapper classesMapper = session.getMapper(ClassesMapper.class);
  List<Classes> all = classesMapper.findAll();
  all.forEach(System.out::println);
}
```

## MyBatis多对多关联查询

<img src=".\mybatis_imgs\image-20211026174411849.png" alt="image-20211026174411849" style="zoom:50%;" />

MyBatis多对多关联查询本质就是两个一对多关联查询。

例如有老师类和班级类：

一个老师对应多个班级，也就是老师类中有一个班级集合属性。

一个班级对应多个老师，也就是班级类中有一个老师集合属性。

### 实体类设计如下：(分解查询也用到该实体类)***

```java
public class Teacher {
  private Integer tid;
  private String tname;
  private List<Classes> classes;
  // 省略getter/setter/toString
}


public class Classes {
  private Integer cid;
  private String className;
  private List<Student> studentList;
  private List<Teacher> teacherList;
  // 省略getter/setter/toString
}
```

在数据库设计中，需要建立中间表，双方与中间表均为一对多关系。

![image-20211026152034008](.\mybatis_imgs\image-20211026152034008.png)

接下来测试查询老师时，将关联的班级集合查询出来。

### 创建持久层接口

```java
public interface TeacherMapper {
  List<Teacher> findAll();
}
```

### 创建映射文件

```xml
<resultMap id="teacherMapper" type="com.itbaizhan.pojo.Teacher">
  <id column="tid" property="tid"></id>
  <result column="tname" property="tname"></result>
  <collection property="classes" column="tid" ofType="com.itbaizhan.pojo.Classes">
    <id column="cid" property="cid"></id>
    <result column="className" property="className"></result>
  </collection>
</resultMap>


<select id="findAll" resultMap="teacherMapper">
   select *
   from teacher
   left join classes_teacher
   on teacher.tid = classes_teacher.tid
   left join classes
   on classes_teacher.cid = classes.cid
</select>
```

### 测试多对多关联查询

```java
@Test
public void testFindAllTeacher() {
  TeacherMapper teacherMapper = session.getMapper(TeacherMapper.class);
  List<Teacher> all = teacherMapper.findAll();
  all.forEach(System.out::println);
}

```

### 反方向的一对多（班级~老师集合~学生集合）

如果**想查询班级时，将关联的老师集合查询出来**，只需要修改班级映射文件的Sql语句和`<resultMap>`即可：

```xml
<resultMap id="classesMapper" type="com.itbaizhan.pojo.Classes">
  <id property="cid" column="cid"></id>
  <result property="className" column="className"></result>
  <!-- 集合列  property：属性名  column：关联列名 ofType：集合的泛型 -->
  <collection property="studentList" column="classId" ofType="com.itbaizhan.pojo.Student">
    <id property="sid" column="sid"></id>
    <result property="name" column="name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
  </collection>、
    
    
    <!-- 集合列  property：属性名  column：关联列名 ofType：集合的泛型 -->
  <collection property="teacherList" column="cid" ofType="com.itbaizhan.pojo.Teacher">
    <id property="tid" column="tid"></id>
    <result property="tname" column="tname"></result>
  </collection>
</resultMap>


<select id="findAll" resultMap="classesMapper">
   select *
   from classes
   left join student
   on classes.cid = student.classId
   left join classes_teacher
   on classes.cid = classes_teacher.cid
   left join teacher
   on classes_teacher.tid = teacher.tid;
</select>
```

## MyBatis分解式查询_一对多

![image-20211026173016040](.\mybatis_imgs\image-20211026173016040.png)

在MyBatis多表查询中，使用连接查询时一个Sql语句就可以查询出所有的数据。如：

```sql
# 查询班级时关联查询出学生
select *
  from classes
  left join student
  on student.classId = classes.cid
```

也可以使用分解式查询，即将一个连接Sql语句分解为多条Sql语句，如：

```sql
# 查询班级时关联查询出学生
select * from classes;
select * from student where classId = 1;
select * from student where classId = 2; 
```

这种写法也叫N+1查询。

> 连接查询：
>
> - 优点：降低查询次数，从而提高查询效率。
> - 缺点：如果查询返回的结果集较多会消耗内存空间。
>
> N+1查询：
>
> - 优点：结果集分步获取，节省内存空间。
> - 缺点：由于需要执行多次查询，相比连接查询效率低。

我们以查询班级时关联查询出学生为例，使用N+1查询：

### 创建每个查询语句的持久层方法

```java
public interface ClassesMapper {
  // 查询所有班级
  List<Classes> findAll();
}


public interface StudentMapper {
  // 根据班级Id查询学生
  List<Student> findByClassId(int classId);
}

```

### 在映射文件中进行配置

```xml
<select id="findAll" resultType="com.itbaizhan.pojo.Classes">
   select * from classes
</select>


<select id="findByClassId" resultType="com.itbaizhan.pojo.Student" parameterType="int">
   select * from student where classId = ${classId}
</select>

```

### 修改主表映射文件中的查询方法

```xml
<!-- 自定义映射关系  -->
<resultMap id="MyClassesMapper" type="com.itbaizhan.pojo.Classes">
  <id property="cid" column="cid"></id>
  <result property="className" column="className"></result>
    
    
  <!-- select：从表查询调用的方法
  column：调用方法时传入的参数字段
   List<Student> findByClassId(int classId); classID 来自 class的cid
-->
    
  <collection property="studentList"
        ofType="com.itbaizhan.pojo.Student"        select="com.itbaizhan.mapper2.StudentMapper2.findByClassId"
        column="cid">
  </collection>
    
     
</resultMap>


<select id="findAll" resultMap="MyClassesMapper">
   select * from classes
</select>

```

### 测试查询方法

```java
@Test
public void testFindAllClasses2(){
  ClassesMapper2 classesMapper2 = session.getMapper(ClassesMapper2.class);
  List<Classes> all = classesMapper2.findAll();
  all.forEach(System.out::println);
}

```

我们可以看到在控制台打印出了多条Sql语句







## MyBatis分解式查询_一对一

查询学生时关联查询出班级也可以使用分解式查询，首先将查询语句分开：

```sql
select * from student;
select * from classes where cid = ?;
```

### 创建每个查询语句的持久层方法

```java
public interface StudentMapper {
   // 查询所有学生
  List<Student> findAll();
}


public interface ClassesMapper {
  // 根据ID查询班级
  Classes findByCid(int cid);
}
```

### 在映射文件中进行配置

```xml
<select id="findAll" resultType="com.itbaizhan.pojo.Student">
   select *
   from student
</select>


<select id="findByCid" resultType="com.itbaizhan.pojo.Classes" parameterType="int">
   select * from classes where cid = ${cid}
</select>
```

### 修改主表映射文件中的查询方法

```xml
<resultMap id="MyStudentMapper" type="com.itbaizhan.pojo.Student">
  <id property="sid" column="sid"></id>
  <result property="name" column="name"></result>
  <result property="age" column="age"></result>
  <result property="sex" column="sex"></result>
    
    <!--  select 分解关联的方法 -->
  <association property="classes"
         javaType="com.itbaizhan.pojo.Classes"
         select="com.itbaizhan.mapper2.ClassesMapper2.findByCid"
         column="classId">
  </association>
</resultMap>


<select id="findAll" resultMap="MyStudentMapper">
   select *
   from student
</select>
```

### 测试查询方法

```java
@Test
public void testFindAllStudent2(){
  StudentMapper2 studentMapper2 = session.getMapper(StudentMapper2.class);
  List<Student> all = studentMapper2.findAll();
  all.forEach(System.out::println);
}
```

## MyBatis延迟加载

<img src=".\mybatis_imgs\src=http___00.imgmini.eastday.com_mobile_20160619_20160619140338_db2f6ab8b9bb54e5dd50f94e9436510b_30.gif&refer=http___00.imgmini.eastday.gif" alt="img" style="zoom:50%;" />

分解式查询又分为两种加载方式：

- 立即加载：在查询主表时就执行所有的Sql语句。
- 延迟加载：又叫懒加载，首先执行主表的查询语句，使用从表数据时才触发从表的查询语句。

延迟加载在获取关联数据时速度较慢，但可以节约资源，即用即取。

### 开启延迟加载

- 设置**所有**的N+1查询都为延迟加载：

  ```xml
  <settings>
      <setting name="lazyLoadingEnabled" value="true"/>
  </settings>
  ```

- 设置**某个方法**为延迟加载：

  在`<association>`、`<collection>`中添加fetchType属性设置加载方式。lazy：延迟加载；eager：立即加载。

### 测试延迟加载

```java
@Test
public void testFindAllClasses2(){
  ClassesMapper2 classesMapper2 = session.getMapper(ClassesMapper2.class);
  List<Classes> all = classesMapper2.findAll();
  all.forEach(System.out::println);
  System.out.println("-------------------------");
  System.out.println(all.get(0).getStudentList());
}
```

由于打印对象时会调用对象的`toString`方法，`toString`方法默认会触发延迟加载的查询，所以我们无法测试出延迟加载的效果。

我们在配置文件设置lazyLoadTriggerMethods属性，**该属性指定对象的什么方法触发延迟加载**，设置为空字符串即可。

```xml
<settings>
  <setting name="lazyLoadTriggerMethods" value=""/>
</settings>
```

> 一般情况下，一对多查询使用延迟加载，一对一查询使用立即加载。



## MyBatis注解开发_环境搭建

<img src=".\mybatis_imgs\image-20211027110547210.png" alt="image-20211027110547210" style="zoom:50%;" />

MyBatis可以使用注解替代映射文件。映射文件的作用就是定义Sql语句，可以在持久层接口上使用@Select/@Delete/@Insert/@Update定义Sql语句，这样就不需要使用映射文件了。

1. 创建maven工程，引入依赖

2. 创建mybatis核心配置文件SqlMapConfig.xml

3. 将log4j.properties文件放入resources中，让控制台打印SQL语句。

4. 创建实体类

5. 创建持久层接口，并在接口方法上定义Sql语句

   ```java
   public interface UserMapper {
     @Select("select * from user")
     List<User> findAll();
   }
   ```

   > 由于注解在方法上方，而方法中就有参数类型和返回值类型，所以使用注解开发不需要定义参数类型和返回值类型
   >
   > 在核心配置文件注册持久层接口，

6. 在核心配置文件注册持久层接口，由于没有映射文件，所以只能采用注册接口或注册包的方法。

   ```xml
   <mappers>
     <package name="com.itbaizhan.mapper"/>
   </mappers>
   ```

7. 测试方法

   ```java
   InputStream is = null;
   SqlSession session = null;
   UserMapper userMapper = null;
   
   @Before
   public void before() throws IOException {
     is = Resources.getResourceAsStream("SqlMapConfig.xml");
     SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
     SqlSessionFactory factory = builder.build(is);
     session = factory.openSession();
     userMapper = session.getMapper(UserMapper.class);
   }
   
   @After
   public void after() throws IOException {
     session.close();
     is.close();
   }
   
   
   
   @Test
   public void testFindAll(){
     List<User> all = userMapper.findAll();
     all.forEach(System.out::println);
   }
   
   ```

## MyBatis注解开发_增删改查

<img src=".\mybatis_imgs\image-20211027110902845.png" alt="image-20211027110902845" style="zoom:33%;" />

接下来写一套基于MyBatis注解的增删改查方法：

```java

//主键回填
@SelectKey(keyColumn = "id", keyProperty = "id", resultType = int.class,before = false, statement = "SELECT LAST_INSERT_ID()")
@Insert("insert into user(username,sex,address) values(#{username},#{sex},#{address})")
void add(User user);


@Update("update user set username = #{username},sex=#{sex},address=#{address} where id = #{id}")
void update(User user);


@Delete("delete from user where id = #{id}")
void delete(int id);


@Select("select * from user where username like #{username}")
List<User> findByUsernameLike(String username);
```

## MyBatis注解开发_动态Sql

<img src=".\mybatis_imgs\image-20211027111253254.png" alt="image-20211027111253254" style="zoom:50%;" />

MyBatis注解开发中有两种方式构建动态Sql：

### 使用脚本标签

将Sql嵌套在`<script>`内即可使用动态Sql标签：

> 建议直接从映射文件中写好标签
>
> 复制到`<script>`内

```java
// 根据任意条件查询
@Select("<script>" +
    "  select * from user\n" +
    "     <where>\n" +
    "       <if test=\"username != null and username.length() != 0\">\n" +
    "         username like #{username}\n" +
    "       </if>\n" +
    "       <if test=\"sex != null and sex.length() != 0\">\n" +
    "         and sex = #{sex}\n" +
    "       </if>\n" +
    "       <if test=\"address != null and address.length() != 0\">\n" +
    "         and address = #{address}\n" +
    "       </if>\n" +
    "     </where>" +
    "</script>")
List<User> findByCondition(User user);
```

### 在方法中构建动态Sql

在MyBatis中有`@SelectProvider`、`@UpdateProvider`、`@DeleteProvider`、`@InsertProvider`注解。当使用这些注解时将不在注解中直接编写SQL，而是调用某个类的方法来生成SQL。

```java
public class UserProvide{
// 生成根据任意条件查询的Sql语句
public String findByConditionSql(User user){
  StringBuffer sb = new StringBuffer("select * from user where 1=1 ");
  if (user.getUsername() != null && user.getUsername().length() != 0){
    sb.append(" and username like #{username} ");
   }
  if (user.getSex() != null && user.getSex().length() != 0){
    sb.append(" and sex = #{sex} ");
   }
  if (user.getAddress() != null && user.getAddress().length() != 0){
    sb.append(" and address = #{address} ");
   }
  return sb.toString();
}
}

 interface UserMapper{
     @SelectProvider(type= UserProvider.class,method="findyByConditionSql")
     List<user> findByCondition(User user);
 }
```



## MyBatis注解开发_自定义映射关系

### POJO属性名与数据库列名不一致时

当POJO属性名与数据库列名不一致时，需要自定义实体类和结果集的映射关系，在MyBatis注解开发中，使用`@Results`**定义并使用**自定义映射，使用`@ResultMap`**使用**自定义映射，用法如下：

```java
// 查询所有用户
@Results(id = "userDiyMapper" ,value = {
  @Result(id = true,property = "id",column = "id"),
  @Result(property = "username",column = "username1"),
  @Result(property = "sex",column = "sex1"),
  @Result(property = "address",column = "address1"),
})
@Select("select * from user")
List<User> findAll();


// 根据id查询
@ResultMap("userDiyMapper")
@Select("select * from user where id = #{id}")
User findById(int id);
```







## MyBatis注解开发_二级缓存

<img src=".\mybatis_imgs\image-20211027183749856.png" alt="image-20211027183749856" style="zoom:50%;" />

MyBatis默认开启一级缓存，接下来我们学习如何在注解开发时使用二级缓存：

1. POJO类实现Serializable接口。

2. 在MyBatis配置文件添加如下设置：

   ```xml
   <settings>
     <setting name="cacheEnabled" value="true"/>
   </settings>
   ```

3. 在持久层接口上方加注解@CacheNamespace(blocking=true)，该接口的所有方法都支持二级缓存。

4. 测试二级缓存

   ```java
   @Test
   public void testCache() throws IOException {
     InputStream is = Resources.getResourceAsStream("SqlMapConfig.xml");
     SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
     SqlSessionFactory factory = builder.build(is);
     SqlSession session1 = factory.openSession();
     SqlSession session2 = factory.openSession();
   
     User user1 = session1.getMapper(UserMapper.class).findById(1);
     System.out.println(user1);
     System.out.println(user1.hashCode());
     session1.commit(); // 清空一次缓存，将数据存到二级缓存
     User user2 = session2.getMapper(UserMapper.class).findById(1);
     System.out.println(user2);
     System.out.println(user2.hashCode());
   
   }
   ```

   

## MyBatis注解开发_一对一关联查询

<img src=".\mybatis_imgs\image-20211027184255173.png" alt="image-20211027184255173" style="zoom:50%;" />

在MyBatis的**注解开发中对于多表查询只支持分解查询，不支持连接查询**。

### 第三步

1. 创建实体类

   ```java
   public class Student {
        private int sid;
        private String name;
        private int age;
        private String sex;
        private Classes classes;
        // 省略getter/setter/toString
      }
   
   public class Classes {
     private int cid;
     private String className;
     private List<Student> students;
     // 省略getter/setter/toString
   }
   ```

2. 创建分解后的查询方法

   ```java
   public interface StudentMapper {
     @Select("select * from student")
     List<Student> findAll();
   }
   
   
   public interface ClassesMapper {
     // 根据id查询班级
     @Select("select * from classes where cid = #{cid}")
     Classes findByCid(Integer cid);
   }
   ```

3. 主表的查询配置自定义映射关系

   ```java
   @Select("select * from student")
   // 自定义映射关系
   @Results(id = "studentMapper",value = {
     @Result(id = true,property = "sid",column = "sid"),
     @Result(property = "name",column = "name"),
     @Result(property = "age",column = "age"),
     @Result(property = "sex",column = "sex"),
     /**
          * property:属性名
          * column:调用从表方法时传入的参数列
          * one:表示该属性是一个对象
          * select：调用的从表方法
          * fetchType：加载方式
          */
     @Result(property = "classes",column = "classId",
         one = @One(select = "com.itbaizhan.mapper.ClassesMapper.findByCid",
               fetchType = FetchType.EAGER))
   })
   List<Student> findAll();
   ```

4. 测试

   ```java
   @Test
   public void findAllStudent(){
     StudentMapper studentMapper = session.getMapper(StudentMapper.class);
     List<Student> all = studentMapper.findAll();
     all.forEach(System.out::println);
   }
   ```



## MyBatis注解开发_一对多关联查询

1. 创建分解后的查询方法

   ```java
   public interface ClassesMapper {
     // 查询所有班级
     @Select("select * from classes")
     List<Classes> findAll();
   }
   
   public interface StudentMapper {
     // 根据班级id查询学生
     @Select("select * from student where classId = #{classId}")
     List<Student> findByClassId(int classId);
   }
   ```

2. 主表的查询配置自定义映射关系

   ```java
   // 查询所有班级
   @Select("select * from classes")
   @Results(id = "classMapper", value = {
     @Result(id = true, property = "cid", column = "cid"),
     @Result(property = "className", column = "className"),
     // many：表示该属性是一个集合
     @Result(property = "studentList", column = "cid",
         many = @Many(select = "com.itbaizhan.mapper.StudentMapper.findByClassId",
                fetchType = FetchType.LAZY))
   })
   List<Classes> findAll();
   ```

3. 测试

   ```java
   @Test
   public void findAllClasses(){
     ClassesMapper classesMapper = session.getMapper(ClassesMapper.class);
     List<Classes> all = classesMapper.findAll();
     all.forEach(System.out::println);
   }
   ```





## 注解开发与映射文件开发的对比

<img src=".\mybatis_imgs\image-20211027185007282.png" alt="image-20211027185007282" style="zoom:50%;" />

MyBatis中更推荐使用映射文件开发，Spring、SpringBoot更推荐注解方式。具体使用要视项目情况而定。它们的优点对比如下：

> 映射文件：
>
> - 代码与Sql语句是解耦的，修改时只需修改配置文件，无需修改源码。
> - Sql语句集中，利于快速了解和维护项目。
> - 级联查询支持连接查询和分解查询两种方式，注解开发只支持分解查询。
>
> 注解：
>
> - 配置简单，开发效率高。
> - 类型安全，在编译期即可进行校验，不用等到运行时才发现错误。






