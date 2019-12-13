[toc]

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
   	 * proxy CGlib根据指定父类生成的代理对象
   	 * method 拦截的方法的参数数组
   	 * args 拦截的方法的参数数组
   	 * methodProxy 方法的代理对象，用于执行父类的方法 
   	 */
   	@Override
   	public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
   		// 创建切面对象
   		MyAspect myAspect = new MyAspect();
   		
   		// 前增强
   		myAspect.check_Permissions();
   		// 目标方法执行
   		Object obj = methodProxy.invokeSuper(proxy, args);
   		// 后增强
   		myAspect.log();
   		return obj;
   	}
   
   }
   
   ```

3. 编写测试类

   ```java
   package com.itheima.cglib;
   
   public class CglibTest {
   	public static void main(String[] args) {
   		//创建代理对象
   		CglibProxy cglibProxy = new CglibProxy();
   		//创建目标对象
   		UserDao userDao = new UserDao();
   		//获取增强后的目标的对象
   		UserDao userDao1 = (UserDao) cglibProxy.createProxy(userDao);
   		//执行方法
   		userDao1.addUser();
   		userDao1.deleteUser();
   	}
   }
   ```

   

## 2、基于代理类的AOP实现

基于代理类的AOP实现，看起来简单，实际上在xml配置上过于繁琐，我个人还是比较倾向使用annotation开发的，毕竟使用annotation开发可以比较轻松的理解其中的逻辑，

接下来就用一个例子来解释：

1. 先创建一个包，包名为com.itheima.factorybean

2. 在该包下创建一个切面类，将之命名为MyAspect.java

   ```java
   package com.itheima.factorybean;
   import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
   
   //切面类
   public class MyAspect implements MethodInterceptor{
       
       @Override
       public Object invoke(MethodInvocation mi) throws Throwable{
           check_Permissions();
           //执行目标方法
           Object obj = mi.proceed();
           
           log();
           return obj;
       }
       private void check_Permissions(){
           System.out.println("模拟检查权限");
       }
       
       public void log(){
           System.out.println("模拟记录日志。。。");
       }
   }
   ```
   
3. 再在该包下创建一个测试类，类名为ProxyFactoryBeanTest

   ```java
   package com.itheima.factorybean;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import com.itheima.jdk.UserDao;
   
   public class ProxyFactoryBeanTest{
       
       public static void main(String[] args){
           String xmlPath = "com/itheima/factorybean/applicationContext.xml";
           ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
           //从spring容器中获得内容
           UserDao userDao = (UserDao)applicationContext.getBean("userDaoProxy");
           //执行方法
           userDao.addUser();
           userDao.deleterUser();
       }
   }
   ```

4. 在然后配置xml文件，将其命名为applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://springframework.org/schema/beans
                              http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
   
   <!--1 配置目标类-->
   <bean id="userDao" class="com.itheima.jdk.UserDaoImp"></bean>
   <!--2 配置切面类-->
   <bean id="myAspect" class="com.itheima.factorybean.MyAspect"></bean>
   <!--3 使用spring代理工厂定义一个名为userDaoProxy的代理对象-->
   <bean id="userDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
   <!--3.1 指定代理实现的接口-->
       <property name="proxyInterfaces" value="com.itheima.jdk.UserDao"></property>
   <!--3.2 指定目标对象-->
       <property name="target" ref="userDao"></property>
   <!--3.3 指定切面，织入环绕通知-->
       <property name="interceptorNames" value="myAspect"></property>
   <!--3.4 指定代理方式，true：使用cglib false(默认)：使用jdk动态代理-->
       <property name="proxyTargetClass" value="true"></property>
   </bean>
   </beans>
          
   ```

   

## 3、AspectJ开发

使用AspectJ开发有两种方式，一种是使用XML配置的方法，另外一种是annotation的方式，

我们先来用xml配置的方式进行AspectJ开发

### AspectJ开发之XML

1. 先创建一个包，包名为com.itheima.aspectj.xml

2. 在创建一个切面My Aspect.java

   ```java
   package com.itheima.aspectj.xml;
   
   import org.aspectj.lang.JoinPoint;
   import org.aspectj.lang.ProceedingJoinPoint;
   
   /*
    *  切面类，在此类中编写通知
    */
   public class MyAspect {
   
   	//前置通知
   	public void myBefore(JoinPoint joinPoint) {
   		System.out.print("前置通知：模拟执行权限检查。。。");
   		System.out.print("  目标类是："+joinPoint.getTarget());
   		System.out.println(",被织入增强处理的目标方法为："+joinPoint.getSignature().getName());
   	}
   	//后置通知
   	public void myAfterReturning(JoinPoint joinPoint) {
   		System.out.println("后置通知：模拟记录日志。。。。，");
   		System.out.println("被织入增强处理的目标方法为："+joinPoint.getSignature().getName());
   	}
   	
   	/**
   	 *  0.环绕通知
   	 *  ProceedingJoinPoint是JoinPoint子接口，表示可以执行目标方法
   	 *  1.必须是Object类型的放回值
   	 *  2.必须接收一个参数，类型为ProceedingJoinPoint
   	 *  3.必须是throws Throwable
   	 *  
   	 */
   	public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
   		//开始
   		System.out.println("环绕开始：执行目标方法之前，模拟开启事务。。。。");
   		//执行当前目标方法
   		Object obj = proceedingJoinPoint.proceed();
   		//结束
   		System.out.println("环绕结束：执行目标方法之后，模拟关闭事务。。。。");
   		return obj;
   	}
   	
   	//异常通知
   	public void myAfterThrowing(JoinPoint joinPoint,Throwable e) {
   		System.out.println("异常通知："+"出错了"+e.getMessage());
   	}
   	
   	//最终通知
   	public void myAfter() {
   		System.out.println("最终通知：模拟方法结束后的释放资源");
   	}
   	
   }
   
   ```

3. 再配置我们的xml文件applicationContext.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:aop="http://www.springframework.org/schema/aop"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
           http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">
           
   		<!-- 1 目标类 -->
   		<bean id="userDao" class="com.itheima.jdk.UserDaoImpl"></bean>
   		<!-- 2 切面 -->
   		<bean id="myAspect" class="com.itheima.aspectj.xml.MyAspect"></bean>
   		
   		<!-- 3 aop 编程 -->
   		<aop:config>
   			<!-- 配置切面 -->
   			<aop:aspect ref="myAspect">
   				<!-- 3.1 配置切入点，通知最后增强哪些方法 -->
   				<aop:pointcut expression="execution(* com.itheima.jdk.*.*(..))" id="myPointCut"/>
   				<!-- 3.2 关联通知Advice和切入点pointCut -->
   				<!-- 3.2.1 配置前置通知 -->
   				<aop:before method="myBefore" pointcut-ref="myPointCut"/>
   				<!-- 3.2.2 后置通知， 在方法返回之后执行，就可以获得返回值
   					returning属性：用于后置通知的第二个参数的名称，类型是Object
   				 -->
   				<aop:after-returning method="myAfterReturning" pointcut-ref="myPointCut" returning="returnVal"/>
   				<!-- 3.2.3 环绕通知 -->
   				<aop:around method="myAround" pointcut-ref="myPointCut"/>
   				<!-- 3.2.4 抛出通知：用于处理程序发生异常 -->
   				<!-- *注意：如果程序没有异常，将不会执行增强 -->
   				<!-- *throwing 属性：用于设置通知第二个参数的名称，类型Throwable -->
   				<aop:after-throwing method="myAfterThrowing" pointcut-ref="myPointCut" throwing="e"/>
   				<!-- 3.2.5 最终通知：无论程序发生任何事情，都将执行 -->
   				<aop:after method="myAfter" pointcut-ref="myPointCut"/>
   				
   			</aop:aspect>
   		</aop:config>
   
   </beans>
   ```

4. 然后再编写我们的测试类来进行测试

   ```java
   package com.itheima.aspectj.xml;
   
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   
   import com.itheima.jdk.UserDao;
   
   public class TestXmlAspectj {
   
   	public static void main(String[] args) {
   		// TODO Auto-generated method stub
   		String xmlPath="com/itheima/aspectj/xml/applicationContext.xml";
   	
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
   		
   		// 1 从spring容器中获得内容
   		UserDao userDao = (UserDao) applicationContext.getBean("userDao");
   		// 2执行方法
   		
   		userDao.addUser();
   		
   	}
   
   }
   
   ```

### AspectJ开发之annotation

1. 先编写一个切面

   ```java
   package com.itheima.aspectj.annotation;
   
   import org.aspectj.lang.JoinPoint;
   import org.aspectj.lang.ProceedingJoinPoint;
   import org.aspectj.lang.annotation.AfterReturning;
   import org.aspectj.lang.annotation.AfterThrowing;
   import org.aspectj.lang.annotation.Around;
   import org.aspectj.lang.annotation.Aspect;
   import org.aspectj.lang.annotation.Before;
   import org.aspectj.lang.annotation.Pointcut;
   import org.springframework.stereotype.Component;
   
   /*
    *  切面类，在此类中编写通知
    */
   @Aspect
   @Component   //spring组件
   public class MyAspect {
   
   	//定义切入点表达式
   	@Pointcut("execution(* com.itheima.jdk.*.*(..))")
   	//使用一个返回值为void，方法体为空的方法来命名切入点
   	private void myPointCut() {
   		
   	}
   	
   	//前置通知
   	@Before("myPointCut()")
   	public void myBefore(JoinPoint joinPoint) {
   		System.out.print("前置通知：模拟执行权限检查。。。");
   		System.out.print("  目标类是："+joinPoint.getTarget());
   		System.out.println(",被织入增强处理的目标方法为："+joinPoint.getSignature().getName());
   	}
   	
   	//后置通知
   	@AfterReturning(value="myPointCut()")
   	public void myAfterReturning(JoinPoint joinPoint) {
   		System.out.println("后置通知：模拟记录日志。。。。，");
   		System.out.println("被织入增强处理的目标方法为："+joinPoint.getSignature().getName());
   	}
   	
   	/**
   	 *  0.环绕通知
   	 *  ProceedingJoinPoint是JoinPoint子接口，表示可以执行目标方法
   	 *  1.必须是Object类型的放回值
   	 *  2.必须接收一个参数，类型为ProceedingJoinPoint
   	 *  3.必须是throws Throwable
   	 *  
   	 */
   	@Around("myPointCut()")
   	public Object myAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
   		//开始
   		System.out.println("环绕开始：执行目标方法之前，模拟开启事务。。。。");
   		//执行当前目标方法
   		Object obj = proceedingJoinPoint.proceed();
   		//结束
   		System.out.println("环绕结束：执行目标方法之后，模拟关闭事务。。。。");
   		return obj;
   	}
   	
   	//异常通知
   	@AfterThrowing(value="myPointCut()",throwing="e")
   	public void myAfterThrowing(JoinPoint joinPoint,Throwable e) {
   		System.out.println("异常通知："+"出错了"+e.getMessage());
   	}
   	
   	//最终通知
   	@AfterThrowing("myPointCut()")
   	public void myAfter() {
   		System.out.println("最终通知：模拟方法结束后的释放资源");
   	}
   	
   }
   
   ```

2. 再

# 四、Spring JDBC

它是 spring 框架中提供的一个对象，是对原始 Jdbc API 对象的简单封装。spring 框架为我们提供了很多的操作模板类。

它主要使用org.springframework.jdbc.core包下的JdbcTemplate方法

1. 先创建一个名为com.itheima.jdbc包，并在该包下创建entity类，该类名为Account.java

   ```java
   package com.itheima.jdbc;
   
   public class Acount {
   	
   	private Integer id;//账户id
   	private String username;//用户名
   	private Double balance;//账户余额
   	public Integer getId() {
   		return id;
   	}
   	public void setId(Integer id) {
   		this.id = id;
   	}
   	public String getUsername() {
   		return username;
   	}
   	public void setUsername(String username) {
   		this.username = username;
   	}
   	public Double getBalance() {
   		return balance;
   	}
   	public void setBalance(Double balance) {
   		this.balance = balance;
   	}
   	
   	@Override
   	public String toString() {
   		return "Acount [id=" + id + ", username=" + username + ", balance=" + balance + "]";
   	}
   	
   	
   }
   
   ```

2. 再编写一个接口

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   public interface acountDao {
   	//添加
   	public int addAccount(Acount account);
   	//更新
   	public int updateAccount(Acount account);
   	//删除
   	public int deleteAccount(int id);
   
   	//查找方法
   	//通过id查询
   	public Acount findAccount(int id);
   	//查询所有账户
   	public List<Acount> findAllAcount();
   }
   
   ```

3. 编写该接口的实现类

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   
   import org.springframework.jdbc.core.BeanPropertyRowMapper;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.jdbc.core.RowMapper;
   
   public class AccountDaoImpl implements acountDao {
   
   	//声明jdbcTemplate属性及其setter方法
   	private JdbcTemplate jdbcTemplate;
   	
   	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
   		this.jdbcTemplate = jdbcTemplate;
   	}
   	//添加账户
   	@Override
   	public int addAccount(Acount account) {
   		// TODO Auto-generated method stub
   		String sql="insert into account(username,balance) value(?,?)";
   		//定义数组来存放SQL语句中的参数
   		Object[] obj = new Object[] {account.getUsername(),account.getBalance()};
   		//num是执行添加，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,obj);
   		return num;
   	}
   	//更新账户
   	@Override
   	public int updateAccount(Acount account) {
   		// TODO Auto-generated method stub
   		String sql="update account set username=? , balance=? where id=?";
   		//定义数组来存放SQL语句中的参数
   		Object[] params = new Object[] {account.getUsername(),account.getBalance(),account.getId()};
   		//num是执行更新，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,params);
   		return num;
   	}
   	//删除账户
   	@Override
   	public int deleteAccount(int id) {
   		// TODO Auto-generated method stub
   		String sql="delete from account where id=?";
   	
   		//num是执行删除，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,id);
   		return num;
   	}
   	@Override
   	public Acount findAccount(int id) {
   		// TODO Auto-generated method stub
   		String sql = "select * from account where id = ?";
   		//创建一个新的rowMapper对象
   		RowMapper<Acount> rowMapper = new BeanPropertyRowMapper<Acount>(Acount.class);
   		//将id 绑定SQL语句中通过RowMapper返回一个Object类型的单行记录
   		return this.jdbcTemplate.queryForObject(sql, rowMapper,id);
   	}
   	@Override
   	public List<Acount> findAllAcount() {
   		// TODO Auto-generated method stub
   		String sql = "select * from account";
   		RowMapper<Acount> rowMapper = new BeanPropertyRowMapper<Acount>(Acount.class);
   		return this.jdbcTemplate.query(sql, rowMapper);
   	}
   
   }
   
   ```

4. 再配置xml文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
   
   	<!-- 1 配置数据源 -->
   	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
   		<!-- 数据库驱动 -->
   		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
   		<!-- 连接数据库的url -->
   		<property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
   		<!-- 连接数据库的用户 -->
   		<property name="username" value="root"></property>
   		<!-- 连接数据库的密码 -->
   		<property name="password" value="123456"></property>
   	</bean>
   	<!-- 2 配置JDBC模块 -->
   	<bean id="jdbcTemlate" class="org.springframework.jdbc.core.JdbcTemplate">
   		<!-- 默认必须使用数据源 -->
   		<property name="dataSource" ref="dataSource"></property>
   		
   	</bean>
   	
   	<!-- 定义id为accountDaoBean -->
   	<bean id="accountDao" class="com.itheima.jdbc.AccountDaoImpl">
   		<!-- 将jdbcTemplate注入到accountDao -->
   		<property name="jdbcTemplate" ref="jdbcTemlate"></property>
   	</bean>
   </beans>
   ```

5. 再编写一个测试类

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   public class jdbcTemplateTest {
   
   	/*
   	 * 1 使用execute()方法建表
   	 */
   //	public static void main(String[] args) {
   //		// 加载配置文件
   //		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   //		// 获取jdbcTemplate实例
   //		JdbcTemplate jdbcTemplate = (JdbcTemplate) applicationContext.getBean("jdbcTemlate");
   //		// 使用execute()方法执行SQL语句，创建用户账户管理表account
   //		jdbcTemplate.execute("create table account(" + "id int primary key auto_increment," + "username varchar(50),"
   //				+ "balance double)");
   //		
   //		System.out.println("账户表account创建成功！！");
   //	}
   
   	/*
   	 * 学习junit4测试
   	 */
   	@Test
   	public void mainTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取jdbcTemplate实例
   		JdbcTemplate jdbcTemplate = (JdbcTemplate) applicationContext.getBean("jdbcTemlate");
   		// 使用execute()方法执行SQL语句，创建用户账户管理表account
   		jdbcTemplate.execute("create table account(" + "id int primary key auto_increment," + "username varchar(50),"
   				+ "balance double)");
   		
   		System.out.println("账户表account创建成功！！");
   	}
   	@Test
   	public void addAccountTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取acountDao实例
   		acountDao acountDao = (acountDao) applicationContext.getBean("accountDao");
   		//创建acount对象，并向acount对象添加数据
   		Acount acount = new Acount();
   //		acount.setUsername("tom1");
   //		acount.setBalance(1000.00);
   		int num = 0;
   		for (int i = 0; i < 10; i++) {
   			acount.setUsername("tomm"+i);
   			acount.setBalance(100.00+i);
   			//执行addAcount()方法
   			num += acountDao.addAccount(acount);
   		}
   		
   		if (num > 0) {
   			System.out.println("成功插入了"+num+"条数据");
   		}else {
   			System.out.println("插入失败");
   		}
   	
   	}
   	@Test
   	public void updateAccountTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		acountDao acountDao = (com.itheima.jdbc.acountDao) applicationContext.getBean("accountDao");
   		//修改十一号数据
   		Acount acount = new Acount();
   		int num = 0;
   		for (int i = 11; i < 20; i++) {
   			acount.setId(i);
   			acount.setBalance(101.00*i);
   			acount.setUsername("tty"+i);
   			System.out.println(acount.getId()+"  "+acount.getBalance()+" "+acount.getUsername());
   			num += acountDao.updateAccount(acount);
   		}
   		
   		if (num > 0) {
   			System.out.println("成功更新"+num+"条数据！！！！");
   		} else {
   			System.out.println("数据更新失败！！！！！");
   		}
   	}
   	@Test
   	public void deleteAccountTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取acountDao实例
   		acountDao acountDao = (acountDao) applicationContext.getBean("accountDao");
   		// 开始删除数据
   		int id;
   		int num;
   		id = num = 0;
   		for (int i = 1; i <= 10; i++) {
   			num += acountDao.deleteAccount(i);
   		}
   //		num = acountDao.deleteAccount(1);
   		if (num > 0 ) {
   			System.out.println("成功删除了"+num+"条数据！！！");
   		}else {
   			System.out.println("一条也没删除成功！！！");
   		}
   	}
   	@Test
   	public void findAccountIDTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		acountDao acountDao = (com.itheima.jdbc.acountDao) applicationContext.getBean("accountDao");
   		int id = 11;
   		Acount acount = acountDao.findAccount(id);
   		System.out.println(acount.getId()+",  "+acount.getUsername()+", "+acount.getBalance());
   	}
   	@Test
   	public void findAllAcountTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		acountDao acountDao = (com.itheima.jdbc.acountDao) applicationContext.getBean("accountDao");
   		List<Acount> acounts = acountDao.findAllAcount();
   		for (Acount acount : acounts) {
   			System.out.println(acount.getId()+",  "+acount.getUsername()+", "+acount.getBalance());
   		}
   	}
   }
   
   ```

   

# 五、Spring事务管理

## spring事务的传播属性

| 名称                      | 值   | 解释 |
| ------------------------- | ---- | ---- |
| PROPAGATION_REQUIRED      | 0 | 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择，也是Spring默认的事务的传播。 |
| PROPAGATION_SUPPORTS      | 1 | 支持当前事务，如果当前没有事务，就以非事务方式执行。 |
| PROPAGATION_MANDATORY     | 2 | 支持当前事务，如果当前没有事务，就抛出异常。 |
| PROPAGATION_REQUIRES_NEW  | 3 | 新建事务，如果当前存在事务，把当前事务挂起。 |
| PROPAGATION_NOT_SUPPORTED | 4 | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。 |
| PROPAGATION_NEVER         | 5 | 以非事务方式执行，如果当前存在事务，则抛出异常。 |
| PROPAGATION_NESTED | 6 | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。 |



## Spring事务的隔离级别

| 名称 | 值   | 解释 |
| ---- | ---- | ---- |
| ISOLATION_DEFAULT | -1   | 这是一个PlatfromTransactionManager默认的隔离级别，使用数据库默认的事务隔离级别。另外四个与JDBC的隔离级别相对应 |
| ISOLATION_READ_UNCOMMITTED | 1    | 这是事务最低的隔离级别，它充许另外一个事务可以看到这个事务未提交的数据。这种隔离级别会产生脏读，不可重复读和幻读。 |
| ISOLATION_READ_COMMITTED | 2    | 保证一个事务修改的数据提交后才能被另外一个事务读取。另外一个事务不能读取该事务未提交的数据。 |
| ISOLATION_REPEATABLE_READ | 4    | 这种事务隔离级别可以防止脏读，不可重复读。但是可能出现幻读。 |
| ISOLATION_SERIALIZABLE | 8    | 这是花费最高代价但是最可靠的事务隔离级别。事务被处理为顺序执行。除了防止脏读，不可重复读外，还避免了幻读。 |

## 基于xml的方式声明事务的例子

1. 先创建一个包，com.itheima.jdbc,并在该包下创建一个接口AccountDao.java

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   public interface AccountDao {
   	//添加
   	public int addAccount(Account account);
   	//更新
   	public int updateAccount(Account account);
   	//删除
   	public int deleteAccount(int id);
   
   	//查找方法
   	//通过id查询
   	public Account findAccount(int id);
   	//查询所有账户
   	public List<Account> findAllAcount();
   	//转账
   	public void transfer(String outUser,String inUser,Double money);
   }
   
   ```

2. 在创建一个entity类，Account.java

   ```java
   package com.itheima.jdbc;
   
   public class Account {
   	
   	private Integer id;//账户id
   	private String username;//用户名
   	private Double balance;//账户余额
   	public Integer getId() {
   		return id;
   	}
   	public void setId(Integer id) {
   		this.id = id;
   	}
   	public String getUsername() {
   		return username;
   	}
   	public void setUsername(String username) {
   		this.username = username;
   	}
   	public Double getBalance() {
   		return balance;
   	}
   	public void setBalance(Double balance) {
   		this.balance = balance;
   	}
   	
   	@Override
   	public String toString() {
   		return "Acount [id=" + id + ", username=" + username + ", balance=" + balance + "]";
   	}
   	
   	
   }
   
   ```

3. 编写一个接口的实现类

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   
   import org.springframework.jdbc.core.BeanPropertyRowMapper;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.jdbc.core.RowMapper;
   
   public class AccountDaoImpl implements AccountDao {
   
   	//声明jdbcTemplate属性及其setter方法
   	private JdbcTemplate jdbcTemplate;
   	
   	public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
   		this.jdbcTemplate = jdbcTemplate;
   	}
   	//添加账户
   	@Override
   	public int addAccount(Account account) {
   		// TODO Auto-generated method stub
   		String sql="insert into account(username,balance) value(?,?)";
   		//定义数组来存放SQL语句中的参数
   		Object[] obj = new Object[] {account.getUsername(),account.getBalance()};
   		//num是执行添加，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,obj);
   		return num;
   	}
   	//更新账户
   	@Override
   	public int updateAccount(Account account) {
   		// TODO Auto-generated method stub
   		String sql="update account set username=? , balance=? where id=?";
   		//定义数组来存放SQL语句中的参数
   		Object[] params = new Object[] {account.getUsername(),account.getBalance(),account.getId()};
   		//num是执行更新，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,params);
   		return num;
   	}
   	//删除账户
   	@Override
   	public int deleteAccount(int id) {
   		// TODO Auto-generated method stub
   		String sql="delete from account where id=?";
   	
   		//num是执行删除，返回的是受SQL语句影响的记录条数
   		int num = this.jdbcTemplate.update(sql,id);
   		return num;
   	}
   	@Override
   	public Account findAccount(int id) {
   		// TODO Auto-generated method stub
   		String sql = "select * from account where id = ?";
   		//创建一个新的rowMapper对象
   		RowMapper<Account> rowMapper = new BeanPropertyRowMapper<Account>(Account.class);
   		//将id 绑定SQL语句中通过RowMapper返回一个Object类型的单行记录
   		return this.jdbcTemplate.queryForObject(sql, rowMapper,id);
   	}
   	@Override
   	public List<Account> findAllAcount() {
   		// TODO Auto-generated method stub
   		String sql = "select * from account";
   		RowMapper<Account> rowMapper = new BeanPropertyRowMapper<Account>(Account.class);
   		return this.jdbcTemplate.query(sql, rowMapper);
   	}
   	/*
   	 * 转账
   	 * inUser : 收款
   	 * outUser:汇款
   	 * money:收款金额
   	 * (non-Javadoc)
   	 * @see com.itheima.jdbc.AccountDao#transfer(java.lang.String, java.lang.String, java.lang.Double)
   	 */
   	@Override
   	public void transfer(String outUser, String inUser, Double money) {
   		// TODO Auto-generated method stub
   		//收款时，收款用户的余额 = 现有余额-扣款金额
   		this.jdbcTemplate.update("update account set balance = balance + ?"
   				+"where username = ?",money,inUser);
   		//模拟系统运行时突发性问题
   //		int i = 1/0;
   		this.jdbcTemplate.update("update account set balance = balance - ?"
   				+"where username = ?",money,outUser);
   	}
   
   }
   
   ```

4. 再配置xml文件

   ```java
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:aop="http://www.springframework.org/schema/aop"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:tx="http://www.springframework.org/schema/tx"
   	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
       	http://www.springframework.org/schema/aop
           http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
           http://www.springframework.org/schema/tx
           http://www.springframework.org/schema/tx/spring-tx-4.3.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-4.3.xsd
           "
           
           >
   
   	<!-- 1 配置数据源 -->
   	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
   		<!-- 数据库驱动 -->
   		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
   		<!-- 连接数据库的url -->
   		<property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
   		<!-- 连接数据库的用户 -->
   		<property name="username" value="root"></property>
   		<!-- 连接数据库的密码 -->
   		<property name="password" value="123456"></property>
   	</bean>
   	<!-- 2 配置JDBC模块 -->
   	<bean id="jdbcTemlate" class="org.springframework.jdbc.core.JdbcTemplate">
   		<!-- 默认必须使用数据源 -->
   		<property name="dataSource" ref="dataSource"></property>
   		
   	</bean>
   	
   	<!-- 定义id为accountDaoBean -->
   	<bean id="accountDao" class="com.itheima.jdbc.AccountDaoImpl">
   		<!-- 将jdbcTemplate注入到accountDao -->
   		<property name="jdbcTemplate" ref="jdbcTemlate"></property>
   	</bean>
   	
   	<!-- 4 配置事务管理器依赖，依赖于数据源 -->
   	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   		<property name="dataSource" ref="dataSource"></property>
   	</bean>
   	<!-- 5 编写通知，对事物进行增强（通知），需要编写对切入点和具体事务细节 -->
   	<tx:advice id="txAdvice" transaction-manager="transactionManager">
   		<tx:attributes>
   			<!-- name:* 表示任意方法名称 -->
   			<tx:method name="*" propagation="REQUIRED" isolation="DEFAULT" read-only="false"/>
   		</tx:attributes>
   	</tx:advice>
   	
   	<!-- 6 编写aop让sprin自动对目标生成代理，需要使用AspectJ表达式 -->
   	<aop:config>
   		<!-- 编写切入点 -->
   		<aop:pointcut expression="execution(* com.itheima.jdbc.*.*(..))" id="txPointCut"/>
   		<!-- 切面：将切入点于通知整合 -->
   		<aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
   	</aop:config>
   	
   </beans>
   ```

5. 再编写测试类，测试此测试类使用junit4测试方法

   ```java
   package com.itheima.jdbc;
   
   import java.util.List;
   
   import org.junit.Test;
   import org.springframework.context.ApplicationContext;
   import org.springframework.context.support.ClassPathXmlApplicationContext;
   import org.springframework.jdbc.core.JdbcTemplate;
   
   public class jdbcTemplateTest {
   
   	/*
   	 * 1 使用execute()方法建表
   	 */
   //	public static void main(String[] args) {
   //		// 加载配置文件
   //		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   //		// 获取jdbcTemplate实例
   //		JdbcTemplate jdbcTemplate = (JdbcTemplate) applicationContext.getBean("jdbcTemlate");
   //		// 使用execute()方法执行SQL语句，创建用户账户管理表account
   //		jdbcTemplate.execute("create table account(" + "id int primary key auto_increment," + "username varchar(50),"
   //				+ "balance double)");
   //		
   //		System.out.println("账户表account创建成功！！");
   //	}
   
   	/*
   	 * 学习junit4测试
   	 */
   	@Test
   	public void mainTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取jdbcTemplate实例
   		JdbcTemplate jdbcTemplate = (JdbcTemplate) applicationContext.getBean("jdbcTemlate");
   		// 使用execute()方法执行SQL语句，创建用户账户管理表account
   		jdbcTemplate.execute("create table account(" + "id int primary key auto_increment," + "username varchar(50),"
   				+ "balance double)");
   		
   		System.out.println("账户表account创建成功！！");
   	}
   	@Test
   	public void addAccountTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取acountDao实例
   		AccountDao acountDao = (AccountDao) applicationContext.getBean("accountDao");
   		//创建acount对象，并向acount对象添加数据
   		Account acount = new Account();
   //		acount.setUsername("tom1");
   //		acount.setBalance(1000.00);
   		int num = 0;
   		for (int i = 0; i < 10; i++) {
   			acount.setUsername("tomm"+i);
   			acount.setBalance(100.00+i);
   			//执行addAcount()方法
   			num += acountDao.addAccount(acount);
   		}
   		
   		if (num > 0) {
   			System.out.println("成功插入了"+num+"条数据");
   		}else {
   			System.out.println("插入失败");
   		}
   	
   	}
   	@Test
   	public void updateAccountTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		AccountDao acountDao = (com.itheima.jdbc.AccountDao) applicationContext.getBean("accountDao");
   		//修改十一号数据
   		Account acount = new Account();
   		int num = 0;
   		for (int i = 11; i < 20; i++) {
   			acount.setId(i);
   			acount.setBalance(101.00*i);
   			acount.setUsername("tty"+i);
   			System.out.println(acount.getId()+"  "+acount.getBalance()+" "+acount.getUsername());
   			num += acountDao.updateAccount(acount);
   		}
   		
   		if (num > 0) {
   			System.out.println("成功更新"+num+"条数据！！！！");
   		} else {
   			System.out.println("数据更新失败！！！！！");
   		}
   	}
   	@Test
   	public void deleteAccountTest() {
   		// 加载配置文件
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		// 获取acountDao实例
   		AccountDao acountDao = (AccountDao) applicationContext.getBean("accountDao");
   		// 开始删除数据
   		int id;
   		int num;
   		id = num = 0;
   		for (int i = 1; i <= 10; i++) {
   			num += acountDao.deleteAccount(i);
   		}
   //		num = acountDao.deleteAccount(1);
   		if (num > 0 ) {
   			System.out.println("成功删除了"+num+"条数据！！！");
   		}else {
   			System.out.println("一条也没删除成功！！！");
   		}
   	}
   	@Test
   	public void findAccountIDTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		AccountDao acountDao = (com.itheima.jdbc.AccountDao) applicationContext.getBean("accountDao");
   		int id = 11;
   		Account acount = acountDao.findAccount(id);
   		System.out.println(acount.getId()+",  "+acount.getUsername()+", "+acount.getBalance());
   	}
   	@Test
   	public void findAllAcountTest() {
   		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
   		AccountDao acountDao = (com.itheima.jdbc.AccountDao) applicationContext.getBean("accountDao");
   		List<Account> acounts = acountDao.findAllAcount();
   		for (Account acount : acounts) {
   			System.out.println(acount.getId()+",  "+acount.getUsername()+", "+acount.getBalance());
   		}
   	}
   }
   
   ```

## 基于annotation方式声明事务的例子

![image-20191211135843076](C:\Users\刘海勋\AppData\Roaming\Typora\typora-user-images\image-20191211135843076.png)

保持再上面的src下创建一个applicationContext-annotation.xml文件

重新配置xml文件，用于annotation开发

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd
    	http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-4.3.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.3.xsd
        "
        
        >

	<!-- 1 配置数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<!-- 数据库驱动 -->
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<!-- 连接数据库的url -->
		<property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
		<!-- 连接数据库的用户 -->
		<property name="username" value="root"></property>
		<!-- 连接数据库的密码 -->
		<property name="password" value="123456"></property>
	</bean>
	<!-- 2 配置JDBC模块 -->
	<bean id="jdbcTemlate" class="org.springframework.jdbc.core.JdbcTemplate">
		<!-- 默认必须使用数据源 -->
		<property name="dataSource" ref="dataSource"></property>
		
	</bean>
	
	<!-- 定义id为accountDaoBean -->
	<bean id="accountDao" class="com.itheima.jdbc.AccountDaoImpl">
		<!-- 将jdbcTemplate注入到accountDao -->
		<property name="jdbcTemplate" ref="jdbcTemlate"></property>
	</bean>
	
	<!-- 4 配置事务管理器依赖，依赖于数据源 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>
	
	<!-- 5  配置注册事务管理器驱动 -->
	<tx:annotation-driven transaction-manager="transactionManager"/>
	
	
</beans>
```

再创建一个TransacationTest.java文件，使用junit4模块测试

```java
package com.itheima.jdbc;

import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

//测试类
public class TransactionTest {

	@Test
	public  void xmlTest() {
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
		AccountDao accountDao = (AccountDao) applicationContext.getBean("accountDao");
		accountDao.transfer("tty11", "tty12", 200.0);
		System.out.println("转账成功");
	}
	
	@Test
	public  void annotationTest() {
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext-annotation.xml");
		AccountDao accountDao = (AccountDao) applicationContext.getBean("accountDao");
		accountDao.transfer("tty11", "tty12", 6000.0);
		System.out.println("转账成功");
	}
}

```



# 六、MyBatis
