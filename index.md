[ toc ]

# Spring

# 前言

关于Java编程中的一些容易忘记的编程概念，以及预备知识，我写在前言。

## “高内聚，低耦合”

并不是内聚越高越好，要看实际需求

## Java反射机制

Java反射是作为入门高级的敲门砖

JAVA反射机制是在运行状态中，对于任意一个实体类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；这种动态获取信息以及动态调用对象方法的功能称为java语言的反射机制。

## 实体类

实体类就是一个拥有Set和Get方法的类。实体类通常总是和数据库之类的（所谓持久层数据）联系在一起。

![image-20191208003041764](C:\Users\刘海勋\AppData\Roaming\Typora\typora-user-images\image-20191208003041764.png)



# 一、什么是Spring

## 0、学习目标

* 掌握Spring的概念和优点
* 理解Spring中的IoC和DI思想
* 掌握ApplicationContext容器的使用
* 掌握属性setter方法注入的实现

## 1、导入相应的jar包（核心容器）

commons-logging-1.2.jar
spring-beans-4.3.6.RELEASE.jar
spring-context-4.3.6.RELEASE.jar
spring-core-4.3.6.RELEASE.jar
spring-expression-4.3.6.RELEASE.jar

## 2、建立Javaweb的动态工程

1. 建立一个包为com.itheima.ioc

2. 建立UserDao的接口

   ```java
   package com.itheima.ioc;
   
   public interface UserDao {
   	public void say();
   }
   
   ```

3. 建立UserDaoImpl的接口实现类

   ```java
   package com.itheima.ioc;
   
   public class UserDaoImpl implements UserDao {
   
   	@Override
   	public void say() {
   		// TODO Auto-generated method stub
   		System.out.println("userDao say hello world!!!");
   	}
   
   }
   
   ```

4. 在src下建立applicationContext.xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
   
   	<!-- 将指定类配置给spring，让spring创建其对象的实例 -->
       <bean id="UserDao" class="com.itheima.ioc.UserDaoImpl">
           <!-- collaborators and configuration for this bean go here -->
       </bean>
       <!-- more bean definitions go here -->
   
   </beans>
   ```
   
   
   
5. 编写一个测试类Testioc

   IoC是控制反转

   ​	**Ioc—Inversion of Control，即“`控制反转`”，不是什么技术，而是一种设计思想。**在Java开发中，**Ioc意味着将你设计好的对象交给容器控制，而不是传统的在你的对象内部直接控制**

   ```java
   package com.itheima.ioc;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class Testioc {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		//1.初始化spring容器加载配置文件按
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		
   		//2.通过容器获取user DAO实例
   		UserDao userDao = (UserDao) applicationContext.getBean("UserDao");
   		//3.调用实例方法
   		userDao.say();
   	}
   
   }
   
   ```

6. 依赖注入**DI **

   

# 二、Spring中的Bean

## 0、学习目标

* 了解Bean的常用属性及其子元素
* 掌握实例化Bean的三种方式
* 熟悉Bean的作用和生命周期
* 掌握Bean的三种装配方式

## 1、Bean的实例化

### 构造器实例化

1. 先创建一个包com.itheima.instance.contructor

2. 在创建一个类Bean1.java

   ```java
   package com.itheima.instance.contructor;
   
   public class Bean1 {
   
   }
   
   ```

   其实里面有无参构造函数

3. 在该包下创建一个beans.xml配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
           
   	<bean id="bean1" class="com.itheima.instance.contructor.Bean1"></bean>
   	
   </beans>
   ```

4. 在创建一个测试类InstanceTest1.java

   ```jav
   package com.itheima.instance.contructor;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class InstanceTest1 {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		//定义配置文件路径
   		String xmlPath = "com/itheima/instance/contructor/beans1.xml";
   		//ApplicationContext在加载配置文件时，对Bean进行实例化
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
   //		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans1.xml");//
   //此处因为xml配置在com.itheima.instance.contructor包下所以要配置文件路径
   //文件路径是以/来分隔，类是由.来分隔
   		Bean1 bean1 = (Bean1) applicationContext.getBean("bean1");
   		System.out.printn(bean1);
   	}
   
   }
   
   ```


### 静态工厂方式实例化

0. 创建一个包com.itheima.instance.static_factory

1. 先建立一个类Bean2.java

   ```java
   package com.itheima.instance.static_factory;
   
   public class Bean2 {
   
   }
   
   ```

2. 在创建一个MyBean2Factory.java

   ```java
   package com.itheima.instance.static_factory;
   
   public class MyBean2Factory {
   
   	public static Bean2 createBean() {
   		return new Bean2();
   	}
   }
   
   ```

3. 在该包下配置xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
           
   	<bean id="bean2" name="haha" class="com.itheima.instance.static_factory.MyBean2Factory" factory-method="createBean"/>
   	
   </beans>
   ```

4. 编写测试类

   ```java
   package com.itheima.instance.static_factory;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class InstanceTest2 {
   
   	public static void main(String[] args) {
   		//定义配置文件路径
   		String xmlPath = "/com/itheima/instance/static_factory/beans2.xml";
   		//ApplicationContext在加在配置文件，对Bean进行实例化
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
   		System.out.println(applicationContext.getBean("haha"));
   	}
   	
   }
   
   ```

    



### 实例工厂方式实例化

0. 先创建一个包com.itheima.instance.factory

1. 创建一个Bean3.java

   ```java
   package com.itheima.instance.factory;
   
   public class Bean3 {
   
   }
   
   ```

   

2. 再创建一个MyBean3Factory.java

   ```java
   package com.itheima.instance.factory;
   
   public class MyBean3Factory {
   	
   	public MyBean3Factory() {
   		System.out.println("bean3工厂实例化中");
   	}
   	
   	public Bean3 createBean() {
   		System.out.println("在createBean中");
   		return new Bean3();
   	}
   }
   
   ```

3. 接着再该包下配置相应的xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
           
   	<!-- 配置工厂 -->
   	<bean id="myBean3Factory" class="com.itheima.instance.factory.MyBean3Factory"/>
   	<!-- 使用factory-bean属性指向配置的实例工厂，使用factory-method属性确定使用工厂中哪个方法 -->
   	<bean id="bean3" factory-bean="myBean3Factory" factory-method="createBean"/>
   </beans>
   ```

4. 编写一个测试类

   ```java
   package com.itheima.instance.factory;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class InstanceTest3 {
   
   	public static void main(String[] args) {
   		//指定配置文件路径
   		String xmlPath = "/com/itheima/instance/factory/beans3.xml";
   		//ApplicationContext在加载配置文件时，对Bean进行实例化
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
   		System.out.println(applicationContext.getBean("bean3"));
   	}
   	
   }
   
   ```

   

## 2、作用域

| 范围                 | 描述                                                         |
| :------------------- | :----------------------------------------------------------- |
| `singleton` （默认） | 每个 Spring IoC容器生成单个bean对象实例                      |
| `prototype`          | 与单例相反，它在每次请求bean时都会生成一个新实例。           |
| `request`            | 将在HTTP请求的完整生命周期内创建并提供单个实例。仅在具有ApplicationContext中有效。 |
|                      |                                                              |
| `session`            | 将在HTTP会话的完整生命周期内创建并提供单个实例。仅在具有ApplicationContext中有效。 |
| `application`        | 将在整个生命周期内创建并提供单个实例`ServletContext`。仅在具有 `ApplicationContext`中有效。 |
| `websocket`          | 将在整个生命周期内创建并提供单个实例`WebSocket`。仅在具有`ApplicationContext`中有效。 |

* singleteon scope

  `singleton`是spring容器中的默认bean范围。它告诉容器每个容器只创建和管理一个bean类的实例。此单个实例存储在此类单例 bean 的缓存中，并且该命名Bean的所有后续请求和引用都将返回缓存的实例。

* prototype scope

  `prototype` 应用程序代码每次对bean发出请求时，scope都会导致创建一个新的bean实例。

  您应该知道，destroy bean生命周期方法不称为`prototype`作用域bean，只调用初始化回调方法。因此，作为开发人员，您负责清理`prototype scope`的bean实例以及其中包含的任何资源。

* request scope

  在`request`范围内，容器为每个HTTP请求创建一个新实例。因此，如果服务器当前正在处理50个请求，那么容器最多可以包含50个单独的bean类实例。对一个实例的任何状态更改都不会对其他实例可见。一旦请求完成，这些实例就会被破坏。

* session scope

  在session scope内，容器为每个HTTP会话创建一个新实例。因此，如果服务器有20个活动会话，那么容器最多可以包含20个单独的bean类实例。单个会话生存期内的所有HTTP请求都可以访问该会话中的同一个单个Bean实例。

  对一个实例的任何状态更改都不会对其他实例可见。一旦会话被销毁/在服务器上结束，这些实例就会被破坏。

* application scope

  在application scope内，容器为每个Web应用程序运行时创建一个实例 它与singleton范围几乎相似，只有两个不同，即

  1. `application`scoped bean是singleton per `ServletContext`，而`singleton`scoped bean是singleton per `ApplicationContext`。请注意，单个应用程序可以有多个应用程序上下文。
  2. `application`scoped bean作为`ServletContext`属性可见。

* websocket scope

  所述WebSocket协议使得客户端和已经选择加入通信客户端远程主机之间的双向通信。WebSocket协议为两个方向的流量提供单个TCP连接。这对于具有同步编辑和多用户游戏的多用户应用程序特别有用。

  在这种类型的Web应用程序中，HTTP仅用于初始握手。如果同意，服务器可以响应HTTP状态 101（切换协议） - 握手请求。如果握手成功，则TCP套接字保持打开状态，客户端和服务器都可以使用它来相互发送消息。

* 自定义线程 scope

  Spring还`thread`使用类提供非默认范围`SimpleThreadScope`。要使用此范围，必须使用类将其注册到容器`CustomScopeConfigurer`。

总结：

Spring框架提供了六个spring bean scope，实例在每个scope内具有不同的生命周期。作为开发人员，我们必须明智地选择任何容器托管bean的scope。此外，当具有不同范围的bean相互引用时，我们必须做出明智的决定。



## 3、注解开发

![image-20191205200840044](C:\Users\刘海勋\AppData\Roaming\Typora\typora-user-images\image-20191205200840044.png)

![image-20191205211027658](C:\Users\刘海勋\AppData\Roaming\Typora\typora-user-images\image-20191205211027658.png)

### 一个注解开发的例子

1. 先编写数据访问层（DAO层）和业务层（Service层）

   1.0  DAO层的注解开发标志为@Repository("XXXX")

   ​		Service层的注解开发标志为@Service("XXXX")

   ​		创建一个包com.itheima.annotation

   1.1  分别创建UserDao.java、UserService.java等接口

   ```java
   package com.itheima.annotation;
   
   public interface UserDao {
   
   	public void save();
   	
   }
   ```

   ```java
   package com.itheima.annotation;
   
   public interface UserService {
   
   	public void save();
   }
   
   ```

2. 再编写UserDaoImpl.java、UserServiceImpl.java的实现类

   ```java
   package com.itheima.annotation;
   
   import org.springframework.stereotype.Repository;
   
   @Repository("userDao")
   public class UserDaoImpl implements UserDao {
   
   	@Override
   	public void save() {
   		// TODO Auto-generated method stub
   		System.out.println("userdao..save...");
   	}
   
   }
   
   ```

   ```java
   package com.itheima.annotation;
   
   import javax.annotation.Resource;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   
   @Service("userService")
   public class UserServiceImpl implements UserService {
   
   	//@Resource(name="userDao")
   	@Autowired
   	private UserDao userDao;
   	@Override
   	public void save() {
   		// TODO Auto-generated method stub
   		//调用userDao中的save()方法
   		this.userDao.save();
   		System.out.println("userservice ...save...");
   	}
   	public void setUserDao(UserDao userDao) {
   		this.userDao = userDao;
   	}
   }
   ```

3. 再写Controller层，controller的注解开发为@Controller("XXXX")

   ```java
   package com.itheima.annotation;
   
   import javax.annotation.Resource;
   
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   
   @Controller("userController")
   public class UserController {
   
   	//@Resource(name="userService")
   	@Autowired
   	private UserService userService;
   	public void save() {
   		this.userService.save();
   		System.out.println("userController...save...");
   	}
   	public void setUserService(UserService userService) {
   		this.userService = userService;
   	}
   }
   
   ```

4. 配置xml文件beans6.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:context="http://www.springframework.org/schema/context"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-4.3.xsd">
   
   	<!-- 使用context命名空间，通知spring扫描指定包下所有的Bean类，进行注解解析  -->
   	<!-- <context:component-scan base-package="com.itheima.annotation"/> -->
   
   	<!-- 使用context命名空间，在配置文件中开启相应的注解处理器 -->
   	<!-- <context:annotation-config/> -->
   	<!-- 分别定义3个Bean实例 -->
   	
   	<!-- <bean id="userDao" class="com.itheima.annotation.UserDaoImpl"></bean>
   	<bean id="userService" class="com.itheima.annotation.UserServiceImpl"></bean>
   	<bean id="userController" class="com.itheima.annotation.UserController"></bean> -->
   	
   	<bean id="userDao" class="com.itheima.annotation.UserDaoImpl"></bean>
   	<bean id="userService" class="com.itheima.annotation.UserServiceImpl" autowire="byName"></bean>
   	<bean id="userController" class="com.itheima.annotation.UserController" autowire="byName"></bean>
   </beans>
   ```

5. 再编写一个测试类

   ```java
   package com.itheima.annotation;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   public class AnnotationAssembleTest {
   	
   	public static void main(String[] args) {
   		//配置文件路径
   		String xmlPath="/com/itheima/annotation/beans6.xml";
   		//加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
   		//获取UserController实例
   		UserController userController = (UserController) applicationContext.getBean("userController");
   		//调用UserController中的save()方法
   		userController.save();
   	}
   }
   
   ```

# 三、Spring AOP

AOP是Spring框架面向切面的编程思想，AOP采用一种称为”横切“的技术，将涉及多业务流程的通用功能抽取并单独封装，形成独立的切面，在合适的时机将这些切面横向切入到业务流程指定的位置中。

## 0、AOP编程思想及术语

AOP是面向切面的编程，其编程思想是把散布于不同业务但功能相同的代码从业务逻辑中抽取出来，封装成独立的模块，这些独立的模块被称为切面，切面的具体功能方法被称为关注点。在业务逻辑执行过程中，AOP会把分离出来的切面和关注点动态切入到业务流程中，这样做的好处是提高了功能代码的重用性和可维护性。

Spring框架提供了@AspectJ注解方法和基于XML架构的方法来实现AOP。

* Aspect

  表示切面。切入业务流程的一个独立模块。

* Join point

  表示连接点，也就是业务流程在运行过程中需要切入的具体位置

* Advice

  表示通知。是切面的具体实现方法。可分为前置通知（Before）、后置通知（AfterReturning）、异常通知（AfterThrowing）、最终通知（After）和环绕通知（Around）五种。实现方法具体属于哪类通知，是在配置文件和注解中指定的。

* Pointcut

  表示切入点。用于定义通知应该切入到哪些连接点上，不同的通知通常需要切入到不同的连接点上

* Target

  表示目标对象。被一个或者多个切面所通知的对象

* Proxy

  表示代理对象。将通知应用到目标对象之后被动态创建的对象。可以简单地理解为，代理对象为目标对象的业务逻辑功能加上被切入的切面所形成的对象。

* Weaving

  表示切入，也称织入。将切面应用到目标对象从而创建一个新的代理对象的过程。这个过程可以发生在编译期、类装载期及运行期。

  

## 1、动态代理

0. 首先创建一个包com.itheima.aspect,里面存放切面

在该包下创建一个MyAspect.java的类，其内容如下：

```java
package com.itheima.aspect;

//切面类：可以存在多个通知Advice(即增强的方法)
public class MyAspect {
	public void check_Permissions() {
		System.out.println("模拟检查权限。。。");
	}
	public void log() {
		System.out.println("模拟记录日志。。。");
	}
}

```



### JDK动态代理

1. 编写一个接口UserDao.java

   ```java
   package com.itheima.jdk;
   
   public interface UserDao {
   	
   	public void addUser();
   	public void deleteUser();
   	
   }
   
   ```

2. 再编写该接口的实现类UserDaoImpl.java

   ```java
   package com.itheima.jdk;
   //目标类
   public class UserDaoImpl implements UserDao {
   
   	@Override
   	public void addUser() {
   		// TODO Auto-generated method stub
   		System.out.println("添加用户");
   	}
   
   	@Override
   	public void deleteUser() {
   		// TODO Auto-generated method stub
   		System.out.println("删除用户");
   	}
   
   }
   
   ```

3. 编写jdk动态代理类jdkProxy.java

   ```java
   package com.itheima.jdk;
   
   import java.lang.reflect.InvocationHandler;
   import java.lang.reflect.Method;
   import java.lang.reflect.Proxy;
   
   import com.itheima.aspect.MyAspect;
   /*
    * JDK代理类
    */
   public class jdkProxy implements InvocationHandler {
   
   	//声明目标类接口
   	public UserDao userDao;
   	
   	//创建代理方法 
   	public Object createProxy(UserDao userDao) {
   		this.userDao = userDao;
   //		1.类加载器
   		ClassLoader classLoader = jdkProxy.class.getClassLoader();
   		//2.被代理对象实现的所有接口
   		Class<?>[] clazz = userDao.getClass().getInterfaces();
   		//3.使用代理类，进行增强，返回的是代理后的对象
   		return Proxy.newProxyInstance(classLoader, clazz, this);
   	}
   	
   	/*
   	 * 所有动态代理类的方法调用，都会交由invoke()方法去处理
   	 * proxy 被代理后的对像
   	 * method 将要被执行的方法 信息(反射)
   	 * args执行方法是需要的参数
   	 */
   	@Override
   	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
   		// TODO Auto-generated method stub
   		//声明切面
   		MyAspect myAspect = new MyAspect();
   		
   		//前增强
   		myAspect.check_Permissions();
   
   		//在目标类上调用方法，并传入参数
   		Object obj = method.invoke(userDao, args);
   		
   		// 后增强
   		myAspect.log();
   		return obj;
   	}
   
   }
   
   ```

4. 再编写测试类，用于测试

   ```java
   package com.itheima.jdk;
   
   public class jdkTest {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		// 创建代理对象
   		jdkProxy jdkProxy = new jdkProxy();
   		// 创建目标对象
   		UserDao userDao = new UserDaoImpl();
   		// 从代理对象中获取增强后的目标对象
   		UserDao userDao1 = (UserDao) jdkProxy.createProxy(userDao);
   		// 执行方法
   		userDao1.addUser();
   		userDao1.deleteUser();
   	}
   
   }
   
   ```

   

### CGLIB动态代理

1. 编写一个实体类UserDao.java

   ```java
   package com.itheima.cglib;
   
   public class UserDao {
   	public void addUser() {
   		System.out.println("添加用户");
   	}
   	public void deleteUser() {
   		System.out.println("删除用户");
   	}
   }
   
   ```

2. 再编写CGLIB动态代理

   ```java
   package com.itheima.cglib;
   
   import java.lang.reflect.Method;
   
   import org.springframework.cglib.proxy.Enhancer;
   import org.springframework.cglib.proxy.MethodInterceptor;
   import org.springframework.cglib.proxy.MethodProxy;
   
   import com.itheima.aspect.MyAspect;
   
   // 代理类
   public class CglibProxy implements MethodInterceptor {
   	
   	//创建代理方法
   	public Object createProxy(Object target) {
   		//创建一个动态类方法
   		Enhancer enhancer = new Enhancer();
   		//确定需要增强的类,设置父类
   		enhancer.setSuperclass(target.getClass());
   		//添加回调函数
   		enhancer.setCallback(this);
   		//返回创建的代理类
   		return enhancer.create();
   		
   	}
   	/*
   	 * proxy CGlib根据指定父类生成的代理
