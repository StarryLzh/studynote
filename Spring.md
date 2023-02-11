# Spring简介

![image-20211124135640588](.\spring_imgs\image-20211124135640588.png)

Spring是一个开源框架，为简化企业级开发而生。它以**IOC**（控制反转）和**AOP**（面向切面）为思想内核，提供了控制层SpringMVC、数据层SpringData、服务层事务管理等众多技术，并可以整合众多第三方框架。

Spring将很多复杂的代码变得优雅简洁，有效的降低代码的耦合度，极大的方便项目的后期维护、升级和扩展。

Spring官网地址：https://spring.io/

# IOC_控制反转思想

<img src="./spring_imgs/image-20211126165759417.png?v=1.0.0" alt="image-20211126165759417" style="zoom:50%;" />

IOC(Inversion of Control) ：程序将创建对象的权利交给框架。

之前在开发过程中，对象实例的创建是由调用者管理的，代码如下：

```java
public interface StudentDao {
  // 根据id查询学生
  Student findById(int id);
}


public class StudentDaoImpl implements StudentDao{
  @Override
  public Student findById(int id) {
    // 模拟从数据库查找出学生
    return new Student(1,"百战程序员","北京");
   }
}


public class StudentService {
  public Student findStudentById(int id){
    // 此处就是调用者在创建对象
    StudentDao studentDao = new StudentDaoImpl();
    return studentDao.findById(1);
   }
}
```

这种写法有两个缺点：

1. 浪费资源：StudentService调用方法时即会创建一个对象，如果不断调用方法则会创建大量StudentDao对象。
2. 代码耦合度高：假设随着开发，我们创建了StudentDao另一个更加完善的实现类StudentDaoImpl2，如果在StudentService中想使用StudentDaoImpl2，则必须修改源码。

而IOC思想是将创建对象的权利交给框架，框架会帮助我们创建对象，分配对象的使用，控制权由程序代码转移到了框架中，控制权发生了反转，这就是Spring的IOC思想。而IOC思想可以完美的解决以上两个问题。

# IOC_自定义对象容器

![image-20211124150748998]( ./spring_imgs/image-20211124150748998.png?v=1.0.0)

## 第四步关键 

接下来我们通过一段代码模拟IOC思想。创建一个集合容器，先将对象创建出来放到容器中，需要使用对象时，只需要从容器中获取对象即可，而不需要重新创建，此时容器就是对象的管理者。

1. 创建实体类

   ```java
   public class Student {
     private int id;
     private String name;
     private String address;
       // 省略getter/setter/构造方法/tostring
   }
   ```

2. 创建Dao接口和实现类

   ```java
   public interface StudentDao {
     // 根据id查询学生
     Student findById(int id);
   }
   
   
   public class StudentDaoImpl implements StudentDao{
     @Override
     public Student findById(int id) {
       // 模拟从数据库查找出学生
       return new Student(1,"百战程序员","北京");
      }
   }
   
   
   public class StudentDaoImpl2 implements StudentDao{
     @Override
     public Student findById(int id) {
       // 模拟根据id查询学生
       System.out.println("新方法！！！");
       return new Student(1,"百战程序员","北京");
      }
   }
   ```

3. 创建配置文件bean.properties，该文件中定义管理的对象

   ```properties
   studentDao=com.itbaizhan.dao.StudentDaoImpl
   ```

4. **创建容器管理类，该类在类加载时读取配置文件，将配置文件中配置的对象全部创建并放入容器中。**

   > 关键终于spring 容器相当于 map 对象
   >
   > 放置 类对象名 与 实现类 （实例化）全名

   ```java
   public class Container {
     static Map<String,Object> map = new HashMap();
   
   
     static {
       // 读取配置文件
       InputStream is = Container.class.getClassLoader().getResourceAsStream("bean.properties");
       Properties properties = new Properties();
       try {
         properties.load(is);
        } catch (IOException e) {
         e.printStackTrace();
        }
   
   
       // 遍历配置文件的所有配置
       Enumeration<Object> keys = properties.keys();
       while (keys.hasMoreElements()){
         String key = keys.nextElement().toString();
         String value = properties.getProperty(key);
         try {
           // 创建对象
           Object o = Class.forName(value).newInstance();
           // 将对象放入集合中
           map.put(key,o);
          } catch (Exception e) {
           e.printStackTrace();
          }
        }
      }
   
   
     // 从容器中获取对象
     public static Object getBean(String key){
       return map.get(key);
      }
   }
   ```

5. 创建Dao对象的调用者StudentService

   ```java
   public class StudentService {
     public Student findStudentById(int id){
       // 从容器中获取对象
       StudentDao studentDao = (StudentDao) Container.getBean("studentDao");
       System.out.println(studentDao.hashCode());
       return studentDao.findById(id);
      }
   }
   ```

6. 测试StudentService

   ```java
   public class Test {
     public static void main(String[] args) {
      StudentService studentService = new StudentService();
      System.out.println(studentService.findStudentById(1));
      System.out.println(studentService.findStudentById(1));
      }
   }
   ```

   > 测试结果：
   >
   > 1. StudentService从容器中每次拿到的都是同一个StudentDao对象，节约了资源。
   >
   > 2. 如果想使用StudentDaoImpl2对象，只需要修改bean.properties的内容为
   >
   >    ```properties
   >    studentDao=com.itbaizhan.dao.StudentDaoImpl2
   >    ```
   >
   >    即可，无需修改Java源码。

# IOC_Spring实现IOC

![image-20211126170010941]( ./spring_imgs/image-20211126170010941.png?v=1.0.0)

接下来我们使用Spring实现IOC，Spring内部也有一个容器用来管理对象。

1. 创建Maven工程，引入依赖

   ```xml
   <dependencies>
     <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.3.13</version>
     </dependency>
     <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
       <scope>test</scope>
     </dependency>
   </dependencies>
   ```

2. 创建POJO类、Dao类和接口

   ```java
   public class Student {
     private int id;
     private String name;
     private String address;
       // 省略getter/setter/构造方法/tostring
   }
   
   
   public interface StudentDao {
     // 根据id查询学生
     Student findById(int id);
   }
   
   
   public class StudentDaoImpl implements StudentDao{
     @Override
     public Student findById(int id) {
       // 模拟从数据库查找出学生
       return new Student(1,"百战程序员","北京");
      }
   }
   ```

3. 编写xml配置文件，配置文件中配置需要Spring帮我们创建的对象。

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
   
   
     <bean id="studentDao" class="com.itbaizhan.dao.StudentDaoImpl"></bean>
     
   </beans>
   ```

4. 测试从Spring容器中获取对象。

   ```java
   public class TestContainer {
     @Test
     public void t1(){
       // 创建Spring容器
       ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
       // 从容器获取对象
       StudentDao studentDao1 = (StudentDao) ac.getBean("studentDao");
       StudentDao studentDao2 = (StudentDao) ac.getBean("studentDao");
       System.out.println(studentDao1.hashCode());
       System.out.println(studentDao2.hashCode());
       System.out.println(studentDao1.findById(1));
      }
   }
   ```

# IOC_Spring容器类型

![image-20211124165406979]( ./spring_imgs/image-20211124165406979.png?v=1.0.0)

### 容器接口

- BeanFactory：BeanFactory是Spring容器中的顶层接口，它可以对Bean对象进行管理。

- ApplicationContext：ApplicationContext是BeanFactory的子接口。它除了继承 BeanFactory的所有功能外，还添加了对国际化、资源访问、事件传播等方面的良好支持。

  ApplicationContext有以下三个常用实现类：

### 容器实现类

- ClassPathXmlApplicationContext：该类可以从项目中读取配置文件
- FileSystemXmlApplicationContext：该类从磁盘中读取配置文件
- AnnotationConfigApplicationContext：使用该类不读取配置文件，而是会读取注解

```java
@Test
public void t2(){
  // 创建spring容器
  //     ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
  ApplicationContext ac = new FileSystemXmlApplicationContext("C:\\Users\\a\\IdeaProjects\\spring_demo\\src\\main\\resources\\bean.xml");
  
  // 从容器中获取对象
  StudentDao userDao = (StudentDao) ac.getBean("studentDao");
  System.out.println(userDao);
  System.out.println(userDao.findById(1));
}
```

# IOC_对象的创建方式

![image-20211126171348088]( ./spring_imgs/image-20211126171348088.png?v=1.0.0)

Spring会帮助我们创建bean，那么它底层是调用什么方法进行创建的呢？

### 使用构造方法

Spring默认使用类的空参构造方法创建bean：

```java
// 假如类没有空参构造方法，将无法完成bean的创建
public class StudentDaoImpl implements StudentDao{
  public StudentDaoImpl(int a){}
  
  @Override
  public Student findById(int id) {
    // 模拟根据id查询学生
    return new Student(1,"百战程序员","北京");
   }
}
```

### 使用工厂类的方法

Spring可以调用工厂类的方法创建bean：

1. 创建工厂类，工厂类提供创建对象的方法：

   ```java
   public class StudentDaoFactory {
     public StudentDao getStudentDao(){
       return new StudentDaoImpl(1);
      }
   }
   ```

2. 在配置文件中配置创建bean的方式为工厂方式。

   ```xml
   <!--  id：工厂对象的id，class：工厂类  -->
   <bean id="studentDaoFactory" class="com.itbaizhan.dao.StudentDaoFactory"></bean>
   <!--  id：bean对象的id，factory-bean：工厂对象的id，factory-method：工厂方法 -->
   <bean id="studentDao" factory-bean="studentDaoFactory" factory-method="getStudentDao"></bean>
   ```

3. 测试

### 使用工厂类的静态方法

Spring可以调用工厂类的静态方法创建bean：

1. 创建工厂类，工厂提供创建对象的静态方法。

   ```java
   public class StudentDaoFactory2 {
     public static StudentDao getStudentDao2() {
       return new StudentDaoImpl();
      }
   }
   ```

2. 在配置文件中配置创建bean的方式为工厂静态方法。

   ```xml
   <!-- id:bean的id  class:工厂全类名 factory-method:工厂静态方法  -->
   <bean id="studentDao" class="com.itbaizhan.dao.StudentDaoFactory2" factory-method="getStudentDao2"></bean>
   ```

3. 测试

---

# IOC_对象的创建策略

![image-20211126171610772]( ./spring_imgs/image-20211126171610772.png?v=1.0.0)

Spring通过配置`<bean>`中的`scope`属性设置对象的创建策略，共有五种创建策略：

### singleton

单例，默认策略。整个项目只会创建一个对象，通过`<bean>`中的`lazy-init`属性可以设置单例对象的创建时机：

> lazy-init="false"(默认)：立即创建，在容器启动时会创建配置文件中的所有Bean对象。
>
> lazy-init="true"：延迟创建，第一次使用Bean对象时才会创建。

配置单例策略：

```xml
<!--   <bean id="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2" scope="singleton" lazy-init="true"></bean>-->
<bean id="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2" scope="singleton" lazy-init="false"></bean>
```

测试单例策略：

```java
// 为Bean对象的类添加构造方法
public StudentDaoImpl2(){
  System.out.println("创建StudentDao！！！");
}


@Test
public void t2(){
  // 创建Spring容器
  ApplicationContext ac = new ClassPathXmlApplicationContext("bean1.xml");
  // 从容器获取对象
  StudentDao studentDao1 = (StudentDao) ac.getBean("studentDao");
  StudentDao studentDao2 = (StudentDao) ac.getBean("studentDao");
  StudentDao studentDao3 = (StudentDao) ac.getBean("studentDao");
  System.out.println(studentDao1.hashCode());
  System.out.println(studentDao2.hashCode());
  System.out.println(studentDao3.hashCode());
}	
```

### prototype

多例，每次从容器中获取时都会创建对象。

```xml
<!-- 配置多例策略 -->
<bean id="studentDao"   class="com.itbaizhan.dao.StudentDaoImpl2" scope="prototype"></bean>
```

### request

每次请求创建一个对象，只在web环境有效。

### session

每次会话创建一个对象，只在web环境有效。

### gloabal-session

一次集群环境的会话创建一个对象，只在web环境有效。





# IOC_对象的销毁时机

<img src=" ./spring_imgs/image-20211126171718292.png?v=1.0.0" alt="image-20211126171718292" style="zoom:50%;" />

对象的创建策略不同，销毁时机也不同：

- singleton：对象随着容器的销毁而销毁。
- prototype：使用JAVA垃圾回收机制销毁对象。
- request：当处理请求结束，bean实例将被销毁。
- session：当HTTP Session最终被废弃的时候，bean也会被销毁掉。
- gloabal-session：集群环境下的session销毁，bean实例也将被销毁。

# IOC_生命周期方法

<img src=" ./spring_imgs/image-20211126174228461.png?v=1.0.0" alt="image-20211126174228461" style="zoom:50%;" />

Bean对象的生命周期包含创建——使用——销毁，Spring可以配置Bean对象在创建和销毁时自动执行的方法：

**init-method**:创建对象时执行的方法 

**destroy-method**:销毁对象时执行的方法

1. 定义生命周期方法

   ```java
   public class StudentDaoImpl2 implements StudentDao{
      // 创建时自动执行的方法
     public void init(){
       System.out.println("创建StudentDao！！！");
      }
   
   
     // 销毁时自动执行的方法
     public void destory(){
       System.out.println("销毁StudentDao！！！");
      }
   }
   ```

2. 配置生命周期方法

   ```xml
   <!-- init-method:创建对象时执行的方法  destroy-method:销毁对象时执行的方法  -->
   <bean id="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2" scope="singleton"
      init-method="init" destroy-method="destory"></bean>
   ```

3. 测试

   ```java
   @Test
   public void t3(){
     // 创建Spring容器
     ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean1.xml");
   
   
     // 销毁Spring容器，ClassPathXmlApplicationContext才有销毁容器的方法
     ac.close();
   }
   ```

   

# IOC_获取Bean对象的方式

> ```java
> ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean1.xml");
> ```

Spring有多种获取容器中对象的方式：

### 通过id/name获取

1. 配置文件

   ```xml
   <bean name="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2"></bean>
   
   
   <bean id="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2"></bean>
   ```

2. 获取对象

   ```java
   StudentDao studentDao = (StudentDao) ac.getBean("studentDao");
   ```

### 通过类型获取

1. 配置文件

   ```xml
   <bean name="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2"></bean>
   ```

2. 获取对象

   ```java
   StudentDao studentDao2 = ac.getBean(StudentDao.class);
   ```

   可以看到使用类型获取不需要强转。

### 通过类型+id/name获取

虽然使用类型获取不需要强转，但如果在容器中有一个接口的多个实现类对象，则获取时会报错，此时需要使用类型+id/name获取

1. 配置文件

   ```xml
   <bean name="studentDao" class="com.itbaizhan.dao.StudentDaoImpl2"></bean>
   
   <bean name="studentDao1" class="com.itbaizhan.dao.StudentDaoImpl"></bean>
   ```

2. 获取对象

   ```java
   StudentDao studentDao2 = ac.getBean("studentDao",StudentDao.class);
   ```

