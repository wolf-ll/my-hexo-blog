---
title: SSM
date: 2024-05-21 14:28:16
auther: 小狼
summary: SSM框架八股
categories: 后端
tags:
  - 框架
---

# SSM

## Spring

### 模块

Spring 5.X版本模块：

<img src="SSM\1.png" alt="Spring5.x主要模块"  />

| **功能模块**            | **功能介绍**                                                 |
| :---------------------- | :----------------------------------------------------------- |
| Core Container          | 核心容器，**主要提供 IoC 依赖注入功能的支持**。在 Spring 环境下使用任何功能都必须基于 **IOC 容器**。 |
| AOP&Aspects             | 面向切面编程                                                 |
| Testing                 | 提供了对 junit 或 TestNG 测试框架的整合。                    |
| Data Access/Integration | 提供了对数据访问/集成的功能。                                |
| Spring MVC              | 提供了面向Web应用程序的集成功能。                            |

#### Core

* **spring-core**：Spring 框架基本的核心工具类。
* **spring-beans**：提供对 bean 的创建、配置和管理等功能的支持。
* **spring-context**：提供对国际化、事件传播、资源加载等功能的支持。
* **spring-expression**：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用。

#### AOP

- **spring-aspects**：该模块为与 AspectJ 的集成提供支持。
- **spring-aop**：提供了面向切面的编程实现。
- **spring-instrument**：提供了为 JVM 添加代理（agent）的功能。 具体来讲，它为 Tomcat 提供了一个织入代理，能够为 Tomcat 传递类文 件，就像这些文件是被类加载器加载的一样。没有理解也没关系，这个模块的使用场景非常有限。

#### Data Access/Integration

* **spring-jdbc**：提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。
* **spring-tx**：提供对事务的支持。
* **spring-orm**：提供对 Hibernate、JPA、iBatis 等 ORM（对象关系映射） 框架的支持。
* **spring-oxm**：提供一个抽象层支撑 OXM(Object-to-XML-Mapping)，例如：JAXB、Castor、XMLBeans、JiBX 和 XStream 等。
* **spring-jms** : 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。

#### Spring Web

- **spring-web**：对 Web 功能的实现提供一些最基础的支持。
- **spring-webmvc**：提供对 Spring MVC 的实现。
- **spring-websocket**：提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。
- **spring-webflux**：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步。

#### Messaging

* **spring-messaging** 是从 Spring4.0 开始新加入的一个模块，主要职责是为 Spring 框架集成一些基础的报文传送应用。

#### Spring Test

Spring 团队提倡测试驱动开发（TDD）。有了控制反转 (IoC)的帮助，单元测试和集成测试变得更简单。

Spring 的测试模块对 JUnit（单元测试框架）、TestNG（类似 JUnit）、Mockito（主要用来 Mock 对象）、PowerMock（解决 Mockito 的问题比如无法模拟 final, static， private 方法）等等常用的测试框架支持的都比较好。

### ==Spring IOC==

**IoC（Inversion of Control:控制反转）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是**将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理**(对象创建控制权由程序转移到外部)。不过， IoC 并非 Spring 特有，在其他语言中也有应用。

- **控制**：指的是对象创建（实例化、管理）的权力
- **反转**：控制权交给外部环境（Spring 框架、IoC 容器）

在 Spring 中， IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value），Map 中存放的是各种对象。IOC容器的作用：

* IOC容器负责对象的创建、初始化等一系列工作，其中包含了数据层和业务层的类对象
* 被创建或被管理的对象在IOC容器中统称为==Bean==
* IOC容器中放的就是一个个的Bean对象

当IOC容器中创建好service和dao对象后，程序能正确执行么?

* 不行，因为service运行需要依赖dao对象
* IOC容器中虽然有service和dao对象，但是service对象和dao对象没有任何关系
* 需要把dao对象交给service,也就是说**要绑定service和dao对象之间的关系**

像这种在容器中建立对象与对象之间的绑定关系就要用到**DI依赖注入**

* 在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

* 如业务层需要依赖数据层，service就要和dao建立依赖关系

介绍完Spring的IOC和DI的概念后，我们会发现这两个概念的最终目标就是:==充分解耦==，具体实现靠:

* 使用IOC容器管理bean（IOC)
* 在IOC容器内将有依赖关系的bean进行关系绑定（DI）
* 最终结果为:使用对象时不仅可以直接从IOC容器中获取，并且获取到的bean已经绑定了所有的依赖关系.

> 总结：IOC 就是一种反转控制的思想， 而 DI 是对 IOC 的一种具体实现。

### IOC在Spring中的实现

resources下添加spring配置文件applicationContext.xml，并完成bean的配置

<img src="SSM\1629734336440.png" alt="1629734336440" style="zoom: 67%;" />

####  bean基础配置(id与class)

对于bean的基础配置，在前面的案例中已经使用过:

```
<bean id="" class=""/>
```

其中，bean标签的功能、使用方式以及id和class属性的作用，我们通过一张图来描述下

<img src="SSM\image-20210729183500978.png" alt="image-20210729183500978" style="zoom: 50%;" />

* bean依赖注入的ref属性指定bean，必须在容器中存在

  <img src="SSM\1629771744003.png" alt="1629771744003" style="zoom: 67%;" />


#### bean实例化

对象已经能交给Spring的IOC容器来创建了，但是容器是如何来创建对象的呢?

就需要研究下`bean的实例化过程`，在这块内容中主要解决两部分内容，分别是

* bean是如何创建的
* 实例化bean的三种方式，`构造方法`,`静态工厂`和`实例工厂`

##### 无参构造

> Spring 底层默认通过反射技术**调用组件类的无参构造器**来创建组件对象，这一点需要注意。如果在需要无参构造器时，没有无参构造器，则会抛出异常。

将类配置到Spring容器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>

</beans>
```

编写运行程序

```java
public class AppForInstanceBook {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        BookDao bookDao = (BookDao) ctx.getBean("bookDao");
        bookDao.save();

    }
}
```

类中提供构造函数测试

在BookDaoImpl类中添加一个无参构造函数，并打印一句话，方便观察结果。

```java
public class BookDaoImpl implements BookDao {
    public BookDaoImpl() {
        System.out.println("book dao constructor is running ....");
    }
    public void save() {
        System.out.println("book dao save ...");
    }

}
```

运行程序，如果控制台有打印构造函数中的输出，说明Spring容器在创建对象的时候也走的是构造函数。将构造函数改成private测试，运行程序，能执行成功,说明内部走的依然是构造函数,能访问到类中的私有构造方法,显而易见Spring底层用的是反射。

##### 静态工厂

在spring的配置文件application.properties中添加以下内容:

```xml
<bean id="orderDao" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>
```

class:工厂类的类全名

factory-mehod:具体工厂类中创建对象的方法名

对应关系如下图:

<img src="SSM\image-20210729195248948.png" alt="image-20210729195248948" style="zoom:80%;" />

在AppForInstanceOrder运行类，使用从IOC容器中获取bean的方法进行运行测试

```java
public class AppForInstanceOrder {
    public static void main(String[] args) {
        ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao = (OrderDao) ctx.getBean("orderDao");
        orderDao.save();

    }
}
```

在工厂的静态方法中，我们除了new对象还可以做其他的一些业务操作，这些操作必不可少。这种方式一般是用来兼容早期的一些老系统，所以==了解为主==。

##### 实例工厂

在spring的配置文件中添加以下内容:

```xml
<bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
<bean id="userDao" factory-method="getUserDao" factory-bean="userFactory"/>
```

实例化工厂运行的顺序是:

* 创建实例化工厂对象,对应的是第一行配置

* 调用对象中的方法来创建bean，对应的是第二行配置

  * factory-bean:工厂的实例对象

  * factory-method:工厂对象中的具体创建对象的方法名,对应关系如下:

    <img src="SSM\image-20210729200203249.png" alt="image-20210729200203249" style="zoom:80%;" />

factory-mehod:具体工厂类中创建对象的方法名

(2)在AppForInstanceUser运行类，使用从IOC容器中获取bean的方法进行运行测试

```java
public class AppForInstanceUser {
    public static void main(String[] args) {
        ApplicationContext ctx = new 
            ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = (UserDao) ctx.getBean("userDao");
        userDao.save();
    }
}
```

实例工厂实例化的方式就已经介绍完了，配置的过程还是比较复杂，所以Spring为了简化这种配置方式就提供了一种叫`FactoryBean`的方式来简化开发。

##### FactoryBean的使用

具体的使用步骤为:

(1)创建一个UserDaoFactoryBean的类，实现FactoryBean接口，重写接口的方法

```java
public class UserDaoFactoryBean implements FactoryBean<UserDao> {
    //代替原始实例工厂中创建对象的方法
    public UserDao getObject() throws Exception {
        return new UserDaoImpl();
    }
    //返回所创建类的Class对象
    public Class<?> getObjectType() {
        return UserDao.class;
    }
}
```

(2)在Spring的配置文件中进行配置

```xml
<bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>
```

(3)AppForInstanceUser运行类不用做任何修改，直接运行

### DI

#### setter注入

1. 对于setter方式注入引用类型的方式之前已经学习过，快速回顾下:

* 在bean中定义引用类型属性，并**提供可访问的==set==方法**

```java
public class BookServiceImpl implements BookService {
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
}
```

* 配置中使用==property==标签==ref==属性注入引用类型对象

```xml
<bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
	<property name="bookDao" ref="bookDao"/>
</bean>

<bean id="bookDao" class="com.itheima.dao.imipl.BookDaoImpl"/>
```

2.注入简单数据类型：和引用类型类似

> 1.在BookDaoImpl类中声明对应的简单数据类型的属性
>
> 2.为这些属性提供对应的setter方法
>
> 3.在applicationContext.xml中配置

```java
public class BookDaoImpl implements BookDao {

    private String databaseName;
    private int connectionNum;

    public void setConnectionNum(int connectionNum) {
        this.connectionNum = connectionNum;
    }

    public void setDatabaseName(String databaseName) {
        this.databaseName = databaseName;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

配置文件中使用property标签注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <property name="databaseName" value="mysql"/>
     	<property name="connectionNum" value="10"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <property name="bookDao" ref="bookDao"/>
        <property name="userDao" ref="userDao"/>
    </bean>
</beans>
```

#### 构造器注入

```java
public class BookServiceImpl implements BookService{
    private BookDao bookDao;
    // 利用构造器传参
    public BookServiceImpl(BookDao bookDao) {
        this.bookDao = bookDao;
    }

    public void save() {
        System.out.println("book service save ...");
        bookDao.save();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
    </bean>
</beans>
```

标签<constructor-arg>中

* name属性对应的值为构造函数中方法形参的参数名，必须要保持一致。

* ref属性指向的是spring的IOC容器中其他bean对象。

**注入简单数据类型**

```java
public class BookDaoImpl implements BookDao {
    private String databaseName;
    private int connectionNum;

    public BookDaoImpl(String databaseName, int connectionNum) {
        this.databaseName = databaseName;
        this.connectionNum = connectionNum;
    }

    public void save() {
        System.out.println("book dao save ..."+databaseName+","+connectionNum);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        <constructor-arg name="databaseName" value="mysql"/>
        <constructor-arg name="connectionNum" value="666"/>
    </bean>
    <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
        <constructor-arg name="bookDao" ref="bookDao"/>
        <constructor-arg name="userDao" ref="userDao"/>
    </bean>
</beans>
```

* 当构造函数中方法的参数名发生变化后，配置文件中的name属性也需要跟着变
* 这两块存在紧耦合，具体该如何解决?

在解决这个问题之前，需要提前说明的是，这个参数名发生变化的情况并不多，所以上面的还是比较主流的配置方式，下面介绍的，大家都以了解为主。

方式一:删除name属性，添加type属性，按照类型注入

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg type="int" value="10"/>
    <constructor-arg type="java.lang.String" value="mysql"/>
</bean>
```

* 这种方式可以解决构造函数形参名发生变化带来的耦合问题
* 但是如果构造方法参数中有类型相同的参数，这种方式就不太好实现了

方式二:删除type属性，添加index属性，按照索引下标注入，下标从0开始

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
    <constructor-arg index="1" value="100"/>
    <constructor-arg index="0" value="mysql"/>
</bean>
```

* 这种方式可以解决参数类型重复问题
* 但是如果构造方法参数顺序发生变化后，这种方式又带来了耦合问题

介绍完两种参数的注入方式，具体我们该如何选择呢?

1. 强制依赖使用构造器进行，使用setter注入有概率不进行注入导致null对象出现
   * 强制依赖指对象在创建的过程中必须要注入指定的参数
2. 可选依赖使用setter注入进行，灵活性强
   * 可选依赖指对象在创建过程中注入的参数可有可无

#### 依赖自动装配

IoC容器根据bean所依赖的资源**在容器中自动查找并注入到bean中的过程称**为自动装配

自动装配只需要修改applicationContext.xml配置文件即可:

(1)将`<property>`标签删除

(2)在`<bean>`标签中添加autowire属性

首先来实现按照类型注入的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byType"/>

</beans>
```

==注意事项:==

* 需要注入属性的类中**对应属性的setter方法不能省略**
* **被注入的对象必须要被Spring的IOC容器管理**
* 按照类型在Spring的IOC容器中如果找到多个对象，会报`NoUniqueBeanDefinitionException`

一个类型在IOC中有多个对象，还想要注入成功，这个时候就需要**按照名称注入**，配置方式为:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.itheima.dao.impl.BookDaoImpl"/>
    <!--autowire属性：开启自动装配，通常使用按类型装配-->
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl" autowire="byName"/>

</beans>
```

==注意事项:==

* **按照名称注入中的名称指的是什么?**

  <img src="SSM\1629806856156.png" alt="1629806856156" style="zoom:80%;" />

  * bookDao是private修饰的，外部类无法直接方访问
  * 外部类只能通过属性的set方法进行访问
  * ==========**对外部类来说，setBookDao方法名，去掉set后首字母小写是其属性名**=============
    * 为什么是去掉set首字母小写?
    * 这个规则是set方法生成的默认规则，set方法的生成是把属性名首字母大写前面加set形成的方法名
  * 所以按照名称注入，其实是和对应的set方法有关，但是**如果按照标准起名称，属性名和set对应的名是一致的**

* 如果按照名称去找对应的bean对象，**找不到则注入Null**

* 当某一个类型在IOC容器中有多个对象，按照名称注入只找其指定名称对应的bean对象，不会报错 

两种方式介绍完后，以后用的**更多的是==按照类型==注入。**

最后对于依赖注入，需要注意一些其他的配置特征:

1. 自动装配**用于引用类型依赖注入**，因为被注入的对象必须要被Spring的IOC容器管理，不能对简单类型进行操作
2. 使用按类型装配时（byType）必须保障容器中**相同类型的bean唯一**，推荐使用
3. 使用按名称装配时（byName）必须保障容器中**具有指定名称的bean**，因**变量名与配置耦合，不推荐使用**
4. 自动装配优先级低于setter注入与构造器注入，**同时出现时自动装配配置失效**

#### 集合注入

下面的配置方式，都是在bookDao的bean标签中使用<property>进行注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl">
        
    </bean>
</beans>
```

##### 注入数组类型数据

```xml
<property name="array">
    <array>
        <value>100</value>
        <value>200</value>
        <value>300</value>
    </array>
</property>
```

##### 注入Map

```xml
<property name="map">
    <map>
        <entry key="country" value="china"/>
        <entry key="province" value="henan"/>
        <entry key="city" value="kaifeng"/>
    </map>
</property>
```

##### 注入Properties类型数据

```xml
<property name="properties">
    <props>
        <prop key="country">china</prop>
        <prop key="province">henan</prop>
        <prop key="city">kaifeng</prop>
    </props>
</property>
```

### BeanFactory 与 FactoryBean 的区别

**BeanFactory**

* BeanFactory**定义了IOC容器的最基本形式**，并提供了IOC容器应遵守的的最基本的接口，也就是SpringIOC所遵守的最底层和最基本的编程规范。
* 它的职责包括：实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。在Spring代码中，BeanFactory只是个接口，并不是IOC容器的具体实现，但是Spring容器给出了很多种实现，如DefaultListableBeanFactory、XmlBeanFactory、ApplicationContext等，都是附加了某种功能的实现。
* BeanFactory

  * 使用BeanFactory创建的容器是延迟加载
  * 使用ApplicationContext创建的容器是立即加载

```java
package org.springframework.beans.factory;
import org.springframework.beans.BeansException;
import org.springframework.core.ResolvableType;
public interface BeanFactory {
    String FACTORY_BEAN_PREFIX = "&";
    Object getBean(String var1) throws BeansException;
    <T> T getBean(String var1, Class<T> var2) throws BeansException;
    <T> T getBean(Class<T> var1) throws BeansException;
    Object getBean(String var1, Object... var2) throws BeansException;
    <T> T getBean(Class<T> var1, Object... var2) throws BeansException;
    boolean containsBean(String var1);
    boolean isSingleton(String var1) throws NoSuchBeanDefinitionException;
    boolean isPrototype(String var1) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String var1, ResolvableType var2) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String var1, Class<?> var2) throws NoSuchBeanDefinitionException;
    Class<?> getType(String var1) throws NoSuchBeanDefinitionException;
    String[] getAliases(String var1);
}
```

**FactoryBean**

* 一般情况下，Spring通过反射机制利用的class属性指定实现类实例化Bean，在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在中提供大量的配置信息。配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案.
* Spring为此提供了一个org.springframework.bean.factory.FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。FactoryBean接口对于Spring框架来说占用重要的地位，Spring自身就提供了70多个FactoryBean的实现。它们隐藏了实例化一些复杂Bean的细节，给上层应用带来了便利。从Spring3.0开始，FactoryBean开始支持泛型，即接口声明改为FactoryBean的形式

```java
package org.springframework.beans.factory;
public interface FactoryBean<T> {
    T getObject() throws Exception;
    Class<?> getObjectType();
    boolean isSingleton();
}
```

在该接口中还定义了以下3个方法：

* T getObject()返回由FactoryBean创建的Bean实例，如果isSingleton()返回true，则该实例会放到Spring容器中单实例缓存池中；
* boolean isSingleton()：返回由FactoryBean创建的Bean实例的作用域是singleton还是prototype；
* Class getObjectType()：返回FactoryBean创建的Bean类型。当配置文件中的class属性配置的实现类是FactoryBean时，通过getBean()方法返回的不是FactoryBean本身，而是FactoryBean#getObject()方法所返回的对象，相当于FactoryBean#getObject()代理了getBean()方法。
  例：如果使用传统方式配置下面Car的时，Car的每个属性分别对应一个元素标签。

### bean生命周期

**创建 Bean 的实例**：Bean 容器首先会找到配置文件中的 Bean 定义，然后使用 Java 反射 API 来创建 Bean 的实例。

**Bean 属性赋值/填充**：为 Bean 设置相关属性和依赖，例如`@Autowired` 等注解注入的对象、`@Value` 注入的值、`setter`方法或构造函数注入依赖和值、`@Resource`注入的各种资源。

**Bean 初始化**： 

- 如果 Bean 实现了 `BeanNameAware` 接口，调用 `setBeanName()`方法，传入 Bean 的名字。
- 如果 Bean 实现了 `BeanClassLoaderAware` 接口，调用 `setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- **给bean对象设置属性**：如果 Bean 实现了 `BeanFactoryAware` 接口，调用 `setBeanFactory()`方法，传入 `BeanFactory`对象的实例。
- 与上面的类似，如果实现了其他 `*.Aware`接口，就调用相应的方法。
- **bean对象初始化之前操作**：如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessBeforeInitialization()` 方法
- 如果 Bean 实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果 Bean 在配置文件中的定义包含 `init-method` 属性，执行指定的方法。
- **bean对象初始化之后操作**：如果有和加载这个 Bean 的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法。

**销毁 Bean**：销毁并不是说要立马把 Bean 给销毁掉，而是把 Bean 的销毁方法先记录下来，将来需要销毁 Bean 或者销毁容器的时候，就调用这些方法去释放 Bean 所持有的资源。 

- 如果 Bean 实现了 `DisposableBean` 接口，执行 `destroy()` 方法。
- 如果 Bean 在配置文件中的定义包含 `destroy-method` 属性，执行指定的 Bean 销毁方法。或者，也可以直接通过`@PreDestroy` 注解标记 Bean 销毁之前执行的方法。

<img src="SSM\spring-bean-lifestyle.png" alt="img" style="zoom:80%;" />

### 注解开发

#### 注解

和 XML 配置文件一样，注解本身并不能执行，注解本身仅仅只是做一个标记，具体的功能是框架检测到注解标记的位置，然后针对这个位置按照注解标记的功能来执行具体操作。本质上：所有一切的操作都是Java代码来完成的，XML和注解只是告诉框架中的Java代码如何执行。

#### 注解开发定义bean（2. 5）

**步骤1:删除原XML配置**

将配置文件中的`<bean>`标签删除掉

```xml
<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
```

**步骤2:Dao上添加注解**

在BookDaoImpl类上添加`@Component`注解

```java
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    public void save() {
        System.out.println("book dao save ..." );
    }
}
```

==注意:@Component注解不可以添加在接口上，因为接口是无法创建对象的。==

XML与注解配置的对应关系:

<img src="SSM\1629990315619.png" alt="1629990315619" style="zoom:80%;" />

**步骤3:配置Spring的注解包扫描**

为了让Spring框架能够扫描到写在类上的注解，需要在配置文件上进行包扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <context:component-scan base-package="com.itheima"/>
</beans>
```

**说明:**

component-scan

* component:组件,**Spring将管理的bean视作自己的一个组件**
* scan:扫描

base-package**指定Spring框架扫描的包路径，它会扫描指定包及其子包中的所有类上的注解。**

* 包路径越多[如:com.itheima.dao.impl]，扫描的范围越小速度越快
* 包路径越少[如:com.itheima],扫描的范围越大速度越慢
* 一般扫描到项目的组织名称即Maven的groupId下[如:com.itheima]即可。

#### 纯注解开发（3.0）

实现思路：将配置文件applicationContext.xml删除掉，使用类来替换。

* Java类（SpringConfig）替换Spring核心配置文件（ApplicationContext）

  <img src="SSM\1630029254372.png" alt="1630029254372" style="zoom:80%;" />

* @Configuration注解用于设定当前类为配置类

* @ComponentScan注解用于设定扫描路径，此注解只能添加一次，多个数据请用数组格式

  ```
  @ComponentScan({com.itheima.service","com.itheima.dao"})
  ```

* 读取Spring核心配置文件初始化容器对象切换为读取Java配置类初始化容器对象

  ```java
  // 加载配置文件初始化容器
  ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");
  // 加载配置类初始化容器
  ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
  ```

### 注解相关问题

#### 将一个类声明为 Bean 的注解有哪些

- `@Component`：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。
- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。
- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。
- `@Controller` : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 `Service` 层返回数据给前端页面。

通过查看源码我们得知，@Controller、@Service、@Repository这三个注解只是**在@Component注解的基础上起了三个新的名字**。

对于Spring使用IOC容器管理这些组件来说没有区别。所以@Controller、@Service、@Repository这三个注解只是给开发人员看的，让我们能够便于分辨组件的作用。

注意：虽然它们**本质上一样**，但是为了代码的可读性，为了程序结构严谨我们肯定不能随便胡乱标记。

#### @Component 和 @Bean 的区别是什么

- `@Component` 注解作用于类，而`@Bean`注解作用于方法。
- `@Component`通常是通过类路径扫描来自动侦测以及自动装配到 Spring 容器中（我们可以使用 `@ComponentScan` 注解定义要扫描的路径从中找出标识了需要装配的类自动装配到 Spring 的 bean 容器中）。**`@Bean` 注解通常是我们在标有该注解的方法中定义产生这个 bean,`@Bean`告诉了 Spring 这是某个类的实例，当我需要用它的时候还给我。**
- `@Bean` 注解比 `@Component` 注解的**自定义性更强**，而且很多地方我们只能通过 `@Bean` 注解来注册 bean。比如当我**们引用第三方库中的类需要装配到 `Spring`容器时，则只能通过 `@Bean`来实现。**

`@Bean`注解使用示例

```java
@Configuration
public class AppConfig {
    @Bean
    public TransferService transferService() {
        return new TransferServiceImpl();
    }

}
```

上面的代码相当于下面的 xml 配置

```xml
<beans>
    <bean id="transferService" class="com.acme.TransferServiceImpl"/>
</beans>
```

@Bean注解的作用是将方法的返回值制作为Spring管理的一个bean对象

```java
@Configuration
public class SpringConfig {
	@Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName("com.mysql.jdbc.Driver");
        ds.setUrl("jdbc:mysql://localhost:3306/spring_db");
        ds.setUsername("root");
        ds.setPassword("root");
        return ds;
    }
}
```

**注意:不能使用`DataSource ds = new DruidDataSource()`**

因为DataSource接口中没有对应的setter方法来设置属性。

#### 注入 Bean 的注解有哪些

Spring 内置的 `@Autowired` 以及 JDK 内置的 `@Resource` 和 `@Inject` 都可以用于注入 Bean。

| Annotation   | Package                            | Source       |
| ------------ | ---------------------------------- | ------------ |
| `@Autowired` | `org.springframework.bean.factory` | Spring 2.5+  |
| `@Resource`  | `javax.annotation`                 | Java JSR-250 |
| `@Inject`    | `javax.inject`                     | Java JSR-330 |

`@Autowired` 和`@Resource`使用的比较多一些。

#### @Autowired 和 @Resource 的区别是什么

`Autowired` 属于 Spring 内置的注解，默认的注入方式为`byType`（根据类型进行匹配），也就是说会**优先根据接口类型去匹配并注入 Bean** （接口的实现类）。

**这会有什么问题呢？** **当一个接口存在多个实现类的话，`byType`这种方式就无法正确注入对象了**，因为这个时候 Spring 会同时找到多个满足条件的选择，默认情况下它自己不知道选择哪一个。

**这种情况下，注入方式会变为 `byName`（根据名称进行匹配），这个名称通常就是类名（首字母小写）。**就比如说下面代码中的 `smsService` 就是我这里所说的名称，这样应该比较好理解了吧。

```java
// smsService 就是我们上面所说的名称
@Autowired
private SmsService smsService;
```

举个例子，`SmsService` 接口有两个实现类: `SmsServiceImpl1`和 `SmsServiceImpl2`，且它们都已经被 Spring 容器所管理。

```java
// 报错，byName 和 byType 都无法匹配到 bean
@Autowired
private SmsService smsService;
// 正确注入 SmsServiceImpl1 对象对应的 bean
@Autowired
private SmsService smsServiceImpl1;
// 正确注入  SmsServiceImpl1 对象对应的 bean
// smsServiceImpl1 就是我们上面所说的名称
@Autowired
@Qualifier(value = "smsServiceImpl1")
private SmsService smsService;
```

我们还是建议通过 `@Qualifier` 注解来显式指定名称而不是依赖变量的名称。

`@Resource`属于 JDK 提供的注解，默认注入方式为 `byName`。如果无法通过名称匹配到对应的 Bean 的话，注入方式会变为`byType`。

简单总结一下：

- `@Autowired` 是 Spring 提供的注解，`@Resource` 是 JDK 提供的注解。
- `Autowired` 默认的注入方式为`byType`（根据类型进行匹配），`@Resource`默认注入方式为 `byName`（根据名称进行匹配）。
- 当一个接口存在多个实现类的情况下，`@Autowired` 和`@Resource`都需要通过名称才能正确匹配到对应的 Bean。`Autowired` 可以通过 `@Qualifier` 注解来显式指定名称，`@Resource`可以通过 `name` 属性来显式指定名称。
- **`@Autowired` 支持在构造函数、方法、字段和参数上使用**。`@Resource` 主要用于字段和方法上的注入，不支持在构造函数或参数上使用。

为什么不需要set方法：

* @Autowired可以写在属性上，也可也写在setter方法上，最简单的处理方式是`写在属性上并将setter方法删除掉`
* 为什么setter方法可以删除呢?
  * **自动装配基于反射设计创建对象并通过暴力反射为私有属性进行设值**
  * 普通反射只能获取public修饰的内容
  * **暴力反射除了获取public修饰的内容还可以获取private修改的内容**
  * 所以此处无需提供setter方法

### 注解读取properties配置文件

`@Value`一般会被用在从properties配置文件中读取内容进行使用，具体如何实现?

**步骤1：resource下准备properties文件**

jdbc.properties

```properties
name=itheima888
```

**步骤2: 使用注解加载properties配置文件**

在配置类上添加`@PropertySource`注解

```java
@Configuration
@ComponentScan("com.itheima")
@PropertySource("jdbc.properties")
public class SpringConfig {
}

```

**步骤3：使用@Value读取配置文件中的内容**

```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao {
    @Value("${name}")
    private String name;
    public void save() {
        System.out.println("book dao save ..." + name);
    }
}
```

### bean作用范围

Spring 中 Bean 的作用域通常有下面几种：

- **singleton** : IoC 容器中只有唯一的 bean 实例。Spring 中的 **bean 默认都是单例的**，是对单例设计模式的应用。
- **prototype** : 每次获取都会创建一个新的 bean 实例。也就是说，**连续 `getBean()` 两次，得到的是不同的 Bean 实例。**
- **request** （仅 Web 应用可用）: 每一次 HTTP 请求都会产生一个新的 bean（请求 bean），该 bean 仅在当前 HTTP request 内有效。
- **session** （仅 Web 应用可用） : 每一次来自新 session 的 HTTP 请求都会产生一个新的 bean（会话 bean），该 bean 仅在当前 HTTP session 内有效。
- **application/global-session** （仅 Web 应用可用）：每个 Web 应用在启动时创建一个 Bean（应用 Bean），该 bean 仅在当前应用启动时间内有效。
- **websocket** （仅 Web 应用可用）：每一次 WebSocket 会话产生一个新的 bean。

**如何配置 bean 的作用域呢？**

xml 方式：

```xml
<bean id="..." class="..." scope="singleton"></bean>
```

注解方式：

```java
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```

### Bean 是线程安全的吗

Spring 框架中的 Bean 是否线程安全，取决于其作用域和状态。

我们这里以最常用的两种作用域 prototype 和 singleton 为例介绍。几乎所有场景的 Bean 作用域都是使用默认的 singleton ，重点关注 singleton 作用域即可。

> prototype 作用域下，每次获取都会创建一个新的 bean 实例，不存在资源竞争问题，所以不存在线程安全问题。singleton 作用域下，IoC 容器中只有唯一的 bean 实例，可能会存在资源竞争问题（取决于 Bean 是否有状态）。**如果这个 bean 是有状态的话，那就存在线程安全问题（有状态 Bean 是指包含可变的成员变量的对象）。**
>
> 不过，大部分 Bean 实际都是无状态（没有定义可变的成员变量）的（比如 Dao、Service），这种情况下， Bean 是线程安全的。

对于有状态单例 Bean 的线程安全问题，常见的有两种解决办法：

1. 在 Bean 中尽量避免定义可变的成员变量。
2. 在类中定义一个 `ThreadLocal` 成员变量，将需要的可变成员变量保存在 `ThreadLocal` 中（推荐的一种方式）。

### ==Spring AOP==

#### 概念

* AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面向对象编程的一种补充和完善，它是通过预编译方式和运行期动态代理方式实现在**不修改源代码的情况下给程序动态统一添加额外功能**的一种技术。

* OOP(Object Oriented Programming)面向对象编程

* 作用：在不修改原始设计的基础上为其进行功能增强。利用Aop可以对业务逻辑的**各个部分进行隔离**，从而使得业务逻辑各个部分之间的**`耦合度`降低**，提高程序的**可重用性**，同时提高可开发效率

> AOP(Aspect-Oriented Programming:面向切面编程)能够将那些**与业务无关，却为业务模块所共同调用的逻辑或责任**（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。
>
> Spring AOP 就是**基于动态代理**的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理，如下图所示：

<img src="SSM\230ae587a322d6e4d09510161987d346.jpeg" alt="SpringAOPProcess"  />

当然你也可以使用 **AspectJ** ！Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。

#### 术语

**1.横切关注点**

从每个方法中抽取出来的**同一类非核心业务**。在同一个项目中，我们可以使用多个横切关注点对相关的方法进行多个不同方面的增强。

这个概念不是语法层面天然存在的，而是**根据附加功能的逻辑上的需要**：有十个附加功能，就有十个横切关注点。

**2.通知**

每一个横切关注点上**要做的事情**都需要写一个方法来实现，这样的方法就叫**通知方法**。

* 前置通知：在被代理的目标方法前执行

* 返回通知：在被代理的目标方法成功结束后执行（寿终正寝）

* 异常通知：在被代理的目标方法异常结束后执行（死于非命）

* 后置通知：在被代理的目标方法最终结束后执行（盖棺定论）

* 环绕通知：使用try…catch…finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所有位置

**3.切面**

封装通知方法的类

**4.目标**

被代理的目标对象

**5.代理**

向目标对象应用通知之后创建的代理对象

**6.连接点**

这也是一个纯逻辑概念，不是语法定义的。

* 把方法排成一排，每一个横切位置看成x轴方向，把方法从上到下执行的顺序看成y轴，x轴和y轴的交叉点就是连接点。
* **程序执行过程中的任意位置**，粒度为执行方法、抛出异常、设置变量等

<img src="SSM\23.png" alt="23" style="zoom: 67%;" />

**7.切入点**

定位连接点的方式。（**匹配连接点的式子**）

如果把连接点看作数据库中的记录，那么切入点就是查询记录的 SQL 语句。

Spring 的 AOP 技术可以通过切入点定位到特定的连接点。切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条件。
连接点范围要比切入点范围大，是切入点的方法也一定是连接点，但是是**连接点的方法就不一定要被增强，所以可能不是切入点。**

### 基于注解的AOP

案例设定：测算接口执行效率，但是这个案例稍微复杂了点，我们对其进行简化。

简化设定：在方法执行前输出当前系统时间。

> 1.导入坐标(pom.xml)
>
> 2.制作连接点(原始操作，Dao接口与实现类)
>
> 3.制作共性功能(通知类与通知)
>
> 4.定义切入点
>
> 5.绑定切入点与通知关系(切面)

* pom.xml添加Spring依赖

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
  </dependencies>
  ```

* 添加BookDao和BookDaoImpl类

  ```java
  public interface BookDao {
      public void save();
      public void update();
  }
  
  @Repository
  public class BookDaoImpl implements BookDao {
  
      public void save() {
          System.out.println(System.currentTimeMillis());
          System.out.println("book dao save ...");
      }
  
      public void update(){
          System.out.println("book dao update ...");
      }
  }
  ```

* 创建Spring的配置类

  ```java
  @Configuration
  @ComponentScan("com.itheima")
  public class SpringConfig {
  }
  ```

* 编写App运行类

  ```java
  public class App {
      public static void main(String[] args) {
          ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
          BookDao bookDao = ctx.getBean(BookDao.class);
          bookDao.save();
      }
  }
  ```

我们要使用SpringAOP的方式在不改变update方法的前提下让其具有打印系统时间的功能。

#### AOP实现

**步骤1:添加依赖**

pom.xml

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

**步骤2:定义接口与实现类**

```
环境准备的时候，BookDaoImpl已经准备好，不需要做任何修改
```

**步骤3:定义通知类和通知**

通知就是将共性功能抽取出来后形成的方法，共性功能指的就是当前系统时间的打印。

```java
public class MyAdvice {
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

类名和方法名没有要求，可以任意。

**步骤4:定义切入点**

BookDaoImpl中有两个方法，分别是save和update，我们**要增强的是update方法**，该如何定义呢?

```java
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**说明:**

* 切入点定义**依托一个不具有实际意义的方法进行，即无参数、无返回值、方法体无实际逻辑。**
* execution及后面编写的内容，后面会有章节专门去学习。

**步骤5:制作切面**

切面是用来描述通知和切入点之间的关系，如何进行关系的绑定?

```java
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")	// 设置切入点方法
    private void pt(){}
    
    @Before("pt()")		// 设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行==位置==**

<img src="SSM\1630148447689.png" alt="1630148447689" style="zoom:80%;" />

**说明:**@Before翻译过来是之前，也就是说**通知会在切入点方法执行之前执行，**除此之前还有其他四种类型，后面会讲。

**步骤6:将通知类配给容器并标识其为切面类**

```java
@Component
@Aspect		// 设置当前类为AOP切面类
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
    
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

**步骤7:开启注解格式AOP功能**

```java
@Configuration
@ComponentScan("com.itheima")
@EnableAspectJAutoProxy		// 开启注解格式AOP功能
public class SpringConfig {
}
```

**步骤8:运行程序**

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        bookDao.update();
    }
}
```

看到在执行update方法之前打印了系统时间戳，说明对原始方法进行了增强，AOP编程成功。

#### 切入点表达式

##### 语法

* 切入点表达式标准格式：动作关键字(访问修饰符  返回值  包名.类/接口名.方法名(参数) 异常名）

对于这个格式，我们不需要硬记，通过一个例子，理解它:

```
execution(public User com.itheima.service.UserService.findById(int))
```

* execution：动作关键字，描述切入点的行为动作，例如execution表示执行到指定切入点
* public:访问修饰符,还可以是public，private等，可以省略
* User：返回值，写返回值类型
* com.itheima.service：包名，多级包使用点连接
* UserService:类/接口名称
* findById：方法名
* int:参数，直接写参数的类型，多个类型用逗号隔开
* 异常名：方法定义中抛出指定异常，可以省略

##### 通配符

我们使用通配符描述切入点，主要的目的就是简化之前的配置，具体都有哪些通配符可以使用?

* `*`:单个独立的任意符号，可以独立出现，也可以作为前缀或者后缀的匹配符出现

  ```
  execution（public * com.itheima.*.UserService.find*(*))
  ```

  匹配com.itheima包下的任意包中的UserService类或接口中所有find开头的带有一个参数的方法

* `..`：多个连续的任意符号，可以独立出现，常用于简化包名与参数的书写

  ```
  execution（public User com..UserService.findById(..))
  ```

  匹配com包下的任意包中的UserService类或接口中所有名称为findById的方法

* `+`：专用于匹配子类类型

  ```
  execution(* *..*Service+.*(..))
  ```

  这个使用率较低，描述子类的，咱们做JavaEE开发，继承机会就一次，使用都很慎重，所以很少用它。*Service+，表示所有以Service结尾的接口的子类。

##### 书写技巧

- 描述切入点通**==常描述接口==**，而不描述实现类,如果描述到实现类，就出现紧耦合了
- 访问控制修饰符针对接口开发均采用public描述（**==可省略访问控制修饰符描述==**）
- 返回值类型对于增删改类使用精准类型加速匹配，对于查询类使用\*通配快速描述
- **==包名==**书写**==尽量不使用..匹配==**，效率过低，常用\*做单个包描述匹配，或精准匹配
- **==接口名/类名==**书写名称与模块相关的**==采用\*匹配==**，例如UserService书写成\*Service，绑定业务层接口名
- **==方法名==**书写以**==动词==**进行**==精准匹配==**，名词采用*匹配，例如getById书写成getBy*,selectAll书写成selectAll
- 参数规则较为复杂，根据业务方法灵活调整
- 通常**==不使用异常==**作为**==匹配==**规则

#### 通知类型

<img src="SSM\1630166147697.png" alt="1630166147697" style="zoom:80%;" />

(1)前置通知，追加功能到方法执行前,类似于在代码1或者代码2添加内容

(2)后置通知,追加功能到方法执行后,不管方法执行的过程中有没有抛出异常都会执行，类似于在代码5添加内容

(3)返回后通知,追加功能到方法执行后，只有方法正常执行结束后才进行,类似于在代码3添加内容，如果方法执行抛出异常，返回后通知将不会被添加

(4)抛出异常后通知,追加功能到方法抛出异常后，只有方法执行出异常才进行,类似于在代码4添加内容，只有方法抛出异常后才会被添加

(5)环绕通知,环绕通知功能比较强大，它可以追加功能到方法执行的前后，这也是比较常用的方式，它可以实现其他四种通知类型的功能，具体是如何实现的，需要我们往下学习。

**各种通知**

如果我们使用环绕通知的话，要根据原始方法的返回值来设置环绕通知的返回值，具体解决方案为:

```java
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
    
    @Pointcut("execution(int com.itheima.dao.BookDao.select())")
    private void pt2(){}
    
    @Before("pt()")
    public void before() {
        System.out.println("before advice ...");
    }
    @After("pt()")
    public void after() {
        System.out.println("after advice ...");
    }
    @AfterReturning("pt2()")	// 返回后通知
    public void afterReturning() {
        System.out.println("afterReturning advice ...");
    }
    
    @Around("pt2()")
    // 环绕通知必须依赖形参ProceedingJoinPoint才能实现对原始方法的调用，进而实现原始方法调用前后同时添加通知
    public Object aroundSelect(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("around before advice ...");
        //表示对原始操作的调用
        Object ret = pjp.proceed();
        System.out.println("around after advice ...");
        return ret;
    }
}
```

**说明:**

​	为什么返回的是Object而不是int的主要原因是Object类型更通用。

​	在环绕通知中是可以对原始方法返回值就行修改的。

### AOP工作流程

**流程1:Spring容器启动**

* 容器启动就需要去加载bean,哪些类需要被加载呢?
* **需要被增强的类，如:BookServiceImpl**
* **通知类，如:MyAdvice**
* 注意此时**bean对象还没有创建成功**

**流程2:读取所有切面配置中的切入点**

* 上面这个例子中有两个切入点的配置，但是第一个`ptx()`并没有被使用，所以不会被读取。

  <img src="SSM/1630151682428.png?lastModify=171635275" alt="1630151682428" style="zoom: 67%;" />

**流程3:初始化bean**

判定bean对应的**类中的方法是否匹配到任意切入点**

* 注意第1步在容器启动的时候，bean对象还没有被创建成功。
* 要被实例化bean对象的类中的方法和切入点进行匹配
* 匹配失败，创建原始对象,如`UserDao`
  * 匹配失败说明不需要增强，直接调用原始对象的方法即可。
* 匹配成功，**创建原始对象（==目标对象==）的==代理==对象,如:`BookDao`**
  * 匹配成功说明需要对其进行增强
  * 对哪个类做增强，这个类对应的对象就叫做目标对象
  * 因为**要对目标对象进行功能增强，而采用的技术是动态代理，所以会为其创建一个代理对象**
  * 最终运行的是**代理对象的方法**，在该方法中会对原始方法进行功能增强

**流程4:获取bean执行方法**

* 获取的bean是原始对象时，调用方法并执行，完成操作
* 获取的bean是代理对象时，**根据代理对象的运行模式运行原始方法与增强的内容**，完成操作

#### 核心概念

在上面介绍AOP的工作流程中，我们提到了两个核心概念，分别是:

* 目标对象(Target)：原始功能去掉共性功能对应的类产生的对象，这种对象是无法直接完成最终工作的
* 代理(Proxy)：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

上面这两个概念比较抽象，简单来说，

目标对象就是要增强的类[如:BookServiceImpl类]对应的对象，也叫原始对象，不能说它不能运行，只能说它在运行的过程中对于要增强的内容是缺失的。

SpringAOP是在不改变原有设计(代码)的前提下对其进行增强的，它的底层采用的是**代理模式**实现的，所以要对原始对象进行增强，就需要对原始对象创建代理对象，**在代理对象中的方法把通知[如:MyAdvice中的method方法]内容加进去，就实现了增强,这就是我们所说的代理(Proxy)。**

### AOP的优点

1. **模块化**: AOP可以将横向关注点与纵向业务逻辑分离，从而实现模块化，使代码更加清晰易懂，易于维护和扩展；

2. **可重用性**: AOP可以将横向关注点作为独立的模块，从而使这些模块可以被多个应用程序共用，提高代码的可重用性；

3. **简化代码**: AOP可以用比传统方法更少的代码来实现同样的功能，从而简化代码，提高代码的可读性和可维护性；

4. **提高程序的灵活性**: AOP可以通过将横向关注点独立出来，使得程序的各个模块之间的耦合度降低，从而提高程序的灵活性，便于进行功能扩展和修改；

5. **提高程序的安全性**: AOP**可以通过将安全控制与业务逻辑分离，提高程序的安全性，减少潜在的安全漏洞。**

----

可以使用@Order注解来控制切面的顺序。在同一个方法上应用多个切面时，可以为每个切面添加不同的@Order值，值越小的切面将先执行，值越大的切面将后执行。如果没有指定@Order值，则默认优先级为0。

### 拦截器和aop的区别

拦截器和AOP在以下四个方面存在区别：

> 定义和用途：拦截器是一种**设计模式**，拦截器可以在方法调用之前、之后或异常发生时插入额外的逻辑，常见于各种编程语言和框架，如Java的Servlet过滤器、Spring的拦截器等。在Java中，拦截器通常与AOP框架结合使用。拦截器可以在方法级别或类级别进行配置，并按照一定的顺序依次执行。
>
> AOP是一种**编程范式**，旨在通过**将跨越多个对象和层的功能（称为“切面”）从业务逻辑中解耦出来，实现横切关注点的复用**。AOP可以在不修改原始代码的情况下，将切面应用于一个或多个目标对象，以增加特定功能，例如**日志记录、事务管理、性能监控**等。
>
> 拦截对象：拦截器主要针对**URL**进行拦截，而AOP针对的是**具体的代码**，能够实现更加复杂的业务逻辑。
>
> 灵活性：AOP更加灵活，可以对方法进行拦截，也可以对类进行拦截，而拦截器只能对特定的URL或者action进行拦截。
>
> 实现方式：拦截器和AOP都是使用**代理模式**实现，但AOP还包含一种特殊的代理，即CGLib代理。这种代理可以针对类进行代理，而不仅仅是对接口进行代理。

总结来说，拦截器和AOP在定义和用途、拦截对象、灵活性和实现方式上存在区别。拦截器主要用于过滤和拦截特定URL或action，而AOP主要用于解耦和复用横切关注点。

### Spring AOP 和 AspectJ AOP 有什么区别

**Spring AOP 属于运行时增强，而 AspectJ 是编译时增强。** Spring AOP 基于代理(Proxying)，而 AspectJ 基于字节码操作(Bytecode Manipulation)。

Spring AOP 已经集成了 AspectJ ，AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单，

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ ，它比 Spring AOP 快很多。

## SpringMVC

SpringMVC是一种**基于Java实现MVC模型**的轻量级Web框架，隶属于Spring框架的一部分，对Servlet进行了封装。

### MVC

MVC 是**模型(Model)、视图(View)、控制器(Controller)**的简写，其核心思想是通过将**业务逻辑、数据、显示**分离来组织代码。

<img src="SSM\image-20210809181452421.png" alt="img"  />

> M：Model，模型层，指工程中的JavaBean，作用是处理数据
>
> JavaBean分为两类：
>
> - 一类称为实体类Bean：专门存储业务数据的，如 Student、User 等
> - 一类称为业务处理 Bean：指 Service 或 Dao 对象，专门用于处理业务逻辑和数据访问。
>
> V：View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据
>
> C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器
>
> MVC的工作流程： 用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果找到相应的View视图，渲染数据后最终响应给浏览器

### SpringMVC 的核心组件

记住了下面这些组件，也就记住了 SpringMVC 的工作原理。

- **`DispatcherServlet`**：**核心的中央处理器**，负责接收请求、分发，并给予客户端响应。
- **`HandlerMapping`**：**处理器映射器**，根据 URL 去匹配查找能处理的 `Handler` ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
- **`HandlerAdapter`**：**处理器适配器**，根据 `HandlerMapping` 找到的 `Handler` ，适配执行对应的 `Handler`；
- **`Handler`**：**请求处理器**，处理实际请求的处理器。
- **`ViewResolver`**：**视图解析器**，根据 `Handler` 返回的逻辑视图 / 视图，解析并渲染真正的视图，并传递给 `DispatcherServlet` 响应客户端

### SpringMVC 工作原理

<img src="SSM\de6d2b213f112297298f3e223bf08f28.png" alt="img"  />

**流程说明（重要）：**

1. 客户端（浏览器）发送请求， `DispatcherServlet`拦截请求。
2. `DispatcherServlet` 根据请求信息调用 `HandlerMapping` 。`HandlerMapping` 根据 URL 去匹配查找能处理的 `Handler`（也就是我们平常说的 `Controller` 控制器） ，并会将请求涉及到的拦截器和 `Handler` 一起封装。
3. `DispatcherServlet` 调用 `HandlerAdapter`适配器执行 `Handler` 。
4. `Handler` 完成对用户请求的处理后，会返回一个 `ModelAndView` 对象给`DispatcherServlet`，`ModelAndView` 顾名思义，包含了数据模型以及相应的视图的信息。`Model` 是返回的数据对象，`View` 是个逻辑上的 `View`。
5. `ViewResolver` 会根据逻辑 `View` 查找实际的 `View`。
6. `DispaterServlet` 把返回的 `Model` 传给 `View`（视图渲染）。
7. 把 `View` 返回给请求者（浏览器）

### Spring 框架用到了哪些设计模式

- **工厂设计模式** : Spring 使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。
- **代理设计模式** : Spring AOP 功能的实现。
- **单例设计模式** : Spring 中的 Bean 默认都是单例的。
- **模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式** : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

### Spring循环依赖-三级缓存

https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html#spring-%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96%E4%BA%86%E8%A7%A3%E5%90%97-%E6%80%8E%E4%B9%88%E8%A7%A3%E5%86%B3

Spring 框架通过使用三级缓存来解决这个问题，确保即使在循环依赖的情况下也能正确创建 Bean。

Spring 中的三级缓存其实就是三个 Map，如下：

```java
// 一级缓存
/** Cache of singleton objects: bean name to bean instance. */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

// 二级缓存
/** Cache of early singleton objects: bean name to bean instance. */
private final Map<String, Object> earlySingletonObjects = new HashMap<>(16);

// 三级缓存
/** Cache of singleton factories: bean name to ObjectFactory. */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);
```

> **一级缓存（singletonObjects）**：存放最终形态的 Bean（已经实例化、属性填充、初始化），单例池，为“Spring 的单例属性”⽽⽣。一般情况我们获取 Bean 都是从这里获取的，但是并不是所有的 Bean 都在单例池里面，例如原型 Bean 就不在里面。
>
> **二级缓存（earlySingletonObjects）**：存放过渡 Bean（半成品，尚未属性填充），也就是三级缓存中`ObjectFactory`产生的对象，与三级缓存配合使用的，可以防止 AOP 的情况下，每次调用`ObjectFactory#getObject()`都是会产生新的代理对象的。
>
> **三级缓存（singletonFactories）**：存放`ObjectFactory`，`ObjectFactory`的`getObject()`方法（最终调用的是`getEarlyBeanReference()`方法）可以生成原始 Bean 对象或者代理对象（如果 Bean 被 AOP 切面代理）。三级缓存只会对单例 Bean 生效。

接下来说一下 Spring 创建 Bean 的流程：

1. 先去 **一级缓存 `singletonObjects`** 中获取，存在就返回；
2. 如果不存在或者对象正在创建中，于是去 **二级缓存 `earlySingletonObjects`** 中获取；
3. 如果还没有获取到，就去 **三级缓存 `singletonFactories`** 中获取，通过执行 `ObjectFacotry` 的 `getObject()` 就可以获取该对象，获取成功之后，从三级缓存移除，并将该对象加入到二级缓存中。

解决循环依赖的流程如下：

- 当 Spring 创建 A 之后，发现 A 依赖了 B ，又去创建 B，B 依赖了 A ，又去创建 A；
- 在 B 创建 A 的时候，那么此时 A 就发生了循环依赖，由于 A 此时还没有初始化完成，因此在 **一二级缓存** 中肯定没有 A；
- 那么此时就去三级缓存中调用 `getObject()` 方法去获取 A 的 **前期暴露的对象** ，也就是调用上边加入的 `getEarlyBeanReference()` 方法，生成一个 A 的 **前期暴露对象**；
- 然后就将这个 `ObjectFactory` 从三级缓存中移除，并且将前期暴露对象放入到二级缓存中，那么 B 就将这个前期暴露对象注入到依赖，来支持循环依赖。

> **最后总结一下 Spring 如何解决三级缓存**：
>
> 在三级缓存这一块，主要记一下 Spring 是如何支持循环依赖的即可，也就是如果发生循环依赖的话，就去 **三级缓存 `singletonFactories`** 中拿到三级缓存中存储的 `ObjectFactory` 并调用它的 `getObject()` 方法来获取这个循环依赖对象的前期暴露对象（虽然还没初始化完成，但是可以拿到该对象在堆中的存储地址了），并且将这个前期暴露对象放到二级缓存中，这样在循环依赖时，就不会重复初始化了！
>
> 不过，这种机制也有一些缺点，比如增加了内存开销（需要维护三级缓存，也就是三个 Map），降低了性能（需要进行多次检查和转换）。并且，还有少部分情况是不支持循环依赖的，比如非单例的 bean 和`@Async`注解的 bean 无法支持循环依赖。

#### @Lazy能解决循环依赖吗

`@Lazy` 用来标识类是否需要懒加载/延迟加载，可以作用在类上、方法上、构造器上、方法参数上、成员变量中。

Spring Boot 2.2 新增了全局懒加载属性，开启后全局 bean 被设置为懒加载，需要时再去创建。

配置文件配置全局懒加载：

```properties
#默认false
spring.main.lazy-initialization=true
```

编码的方式设置全局懒加载：

```java
SpringApplication springApplication=new SpringApplication(Start.class);
springApplication.setLazyInitialization(false);
springApplication.run(args);
```

如非必要，尽量不要用全局懒加载。**全局懒加载会让 Bean 第一次使用的时候加载会变慢，并且它会延迟应用程序问题的发现**（当 Bean 被初始化时，问题才会出现）。

如果一个 Bean 没有被标记为懒加载，那么它会在 Spring IoC 容器启动的过程中被创建和初始化。如果一个 Bean 被标记为懒加载，那么**它不会在 Spring IoC 容器启动时立即实例化，而是在第一次被请求时才创建**。这可以帮助减少应用启动时的初始化时间，也可以用来解决循环依赖问题。

循环依赖问题是如何通过`@Lazy` 解决的呢？这里举一个例子，比如说有两个 Bean，A 和 B，他们之间发生了循环依赖，那么 A 的构造器上添加 `@Lazy` 注解之后（延迟 Bean B 的实例化），加载的流程如下：

- 首先 Spring 会去创建 A 的 Bean，创建时需要注入 B 的属性；
- 由于在 A 上标注了 `@Lazy` 注解，因此 **Spring 会去创建一个 B 的代理对象，将这个代理对象注入到 A 中的 B 属性；**
- 之后开始执行 B 的实例化、初始化，在注入 B 中的 A 属性时，此时 A 已经创建完毕了，就可以将 A 给注入进去。

通过 `@Lazy` 就解决了循环依赖的注入， **关键点就在于对 A 中的属性 B 进行注入时，注入的是 B 的代理对象，因此不会循环依赖。**

之前说的发生循环依赖是因为在对 A 中的属性 B 进行注入时，注入的是 B 对象，此时又会去初始化 B 对象，发现 B 又依赖了 A，因此才导致的循环依赖。

一般是不建议使用循环依赖的，但是如果项目比较复杂，可以使用 `@Lazy` 解决一部分循环依赖的问题。

## Rest

* ==REST==（Representational State Transfer），表现形式状态转换,它是一种**软件架构==风格==**

  当我们想表示一个网络资源的时候，可以使用两种方式:

  * 传统风格资源描述形式
    * `http://localhost/user/getById?id=1` 查询id为1的用户信息
    * `http://localhost/user/saveUser` 保存用户信息
  * REST风格描述形式
    * `http://localhost/user/1` 
    * `http://localhost/user`

传统方式一般是一个请求url对应一种操作，这样做不仅麻烦，也不安全。查看REST风格的描述，你会发现请求地址变的简单了，并且光看请求URL并不是很能猜出来该URL的具体功能

所以REST的优点有:

- **隐藏资源的访问行为**，无法通过地址得知对资源是何种操作
- 书写简化

根据REST风格对资源进行访问称为==RESTful==。

### 传递路径参数

前端发送请求的时候使用:`http://localhost/users/1`,路径中的`1`就是我们想要传递的参数。

后端获取参数，需要做如下修改:

* 修改@RequestMapping的value属性，将其中修改为`/users/{id}`，目的是和路径匹配
* 在方法的形参前添加@PathVariable注解：绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应

<img src="SSM\1630506231379.png" alt="1630506231379" style="zoom:80%;" />

### 修改

```java
@Controller
public class UserController {
    //设置当前请求方法为PUT，表示REST风格中的修改操作
    @RequestMapping(value = "/users",method = RequestMethod.PUT)
    @ResponseBody
    public String update(@RequestBody User user) {
        System.out.println("user update..." + user);
        return "{'module':'user update'}";
    }
}
```

- 将请求路径更改为`/users`

  - 访问该方法使用 PUT: `http://localhost/users`

- 访问并携带参数:

  <img src="SSM\1630506507096.png" alt="1630506507096" style="zoom:80%;" />

关于接收参数，我们学过三个注解`@RequestBody`、`@RequestParam`、`@PathVariable`,这三个注解之间的区别和应用分别是什么?

* 区别
  * @RequestParam用于接收url地址传参或表单传参
  * @RequestBody用于接收json数据
  * @PathVariable用于接收路径参数，使用{参数名称}描述路径参数
* 应用
  * 后期开发中，发送请求参数超过1个时，以json格式为主，@RequestBody应用较广
  * 如果发送非json格式数据，选用@RequestParam接收请求参数
  * 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值

### RESTful快速开发

做完了RESTful的开发，你会发现==好麻烦==，麻烦在哪?

<img src="SSM\1630507339724.png" alt="1630507339724" style="zoom:80%;" />

问题1：每个方法的@RequestMapping注解中都定义了访问路径/books，重复性太高。

问题2：每个方法的@RequestMapping注解中都要使用method属性定义请求方式，重复性太高。

问题3：每个方法响应json都需要加上@ResponseBody注解，重复性太高。

对于上面所提的这三个问题，具体该如何解决?

```java
@RestController // 设置当前控制器类为RESTful风格，等价于@Controller + ReponseBody
@RequestMapping("/books")	// 将@RequestMapping提到类上面，用来定义所有方法共同的访问路径。
public class BookController {
    
	//@RequestMapping(method = RequestMethod.POST)
    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save..." + book);
        return "{'module':'book save'}";
    }

    //@RequestMapping(value = "/{id}",method = RequestMethod.DELETE)
    // 使用@GetMapping  @PostMapping  @PutMapping  @DeleteMapping代替requestmapping
    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("book delete..." + id);
        return "{'module':'book delete'}";
    }

    //@RequestMapping(method = RequestMethod.PUT)
    @PutMapping
    public String update(@RequestBody Book book){
        System.out.println("book update..." + book);
        return "{'module':'book update'}";
    }

    //@RequestMapping(value = "/{id}",method = RequestMethod.GET)
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("book getById..." + id);
        return "{'module':'book getById'}";
    }

    //@RequestMapping(method = RequestMethod.GET)
    @GetMapping
    public String getAll(){
        System.out.println("book getAll...");
        return "{'module':'book getAll'}";
    } 
}
```

## Maven

### 分模块开发

分模块开发：将原始模块按照功能拆分成若干个子模块，方便模块间的相互调用，接口共享。

<img src="SSM\1630768869208.png" alt="1630768869208" style="zoom: 50%;" />

对于项目的拆分，大致会有如下几个步骤:

(1) 创建Maven模块

(2) 书写模块代码：分模块开发需要先针对模块功能进行设计，再进行编码。不会先将工程开发完毕，然后进行拆分。拆分方式可以按照功能拆也可以按照模块拆。

(3)通过maven指令安装模块到本地仓库(install 指令)，在不同模块间引入依赖

团队内部开发需要发布模块功能到团队内部可共享的仓库中(私服)

### 依赖管理

依赖是具有传递性的:

<img src="SSM\1630853726532.png" alt="1630853726532" style="zoom: 50%;" />

**说明:**A代表自己的项目；B,C,D,E,F,G代表的是项目所依赖的jar包；D1和D2 E1和E2代表是相同jar包的不同版本

(1) A依赖了B和C,B和C有分别依赖了其他jar包，所以在A项目中就可以使用上面所有jar包，这就是所说的依赖传递

(2) 依赖传递有直接依赖和间接依赖

* 相对于A来说，A直接依赖B和C,间接依赖了D1,E1,G，F,D2和E2
* 相对于B来说，B直接依赖了D1和E1,间接依赖了G
* 直接依赖和间接依赖是一个相对的概念

(3)因为有依赖传递的存在，就会导致jar包在依赖的过程中出现冲突问题，Maven是如何解决冲突的?

如果想避免传递依赖：

**方案1-可选依赖**：可选依赖指对外隐藏当前所依赖的资源---不透明<optional>true</optional>

**方案2-排除依赖**：排除依赖指主动断开依赖的资源，被排除的资源无需指定版本---不需要

### 聚合和继承

#### 聚合

* 所谓聚合：将多个模块组织成一个整体，同时进行项目构建的过程称为聚合
* 聚合工程：通常是一个不具有业务功能的"空"工程（有且仅有一个pom文件）
* 作用：使用聚合工程可以将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建
  * 当工程中某个模块发生更新（变更）时，必须保障工程中与已更新模块关联的模块同步更新，此时可以使用聚合工程来解决批量模块同步构建的问题。

聚合工程管理的项目在进行运行的时候，会按照项目与项目之间的依赖关系来自动决定执行的顺序和配置的顺序无关。

#### 继承

* 所谓继承：描述的是两个工程间的关系，与java中的继承相似，子工程可以继承父工程中的配置信息，常见于依赖关系的继承。
* 作用：简化配置；减少版本冲突

步骤1:创建一个空的Maven项目并将其打包方式设置为pom

步骤2:在子项目中设置其父工程

步骤3:优化子项目共有依赖导入问题（将子项目共同使用的jar包都抽取出来，维护在父项目的pom.xml中）

步骤4:优化子项目依赖版本问题

**父工程主要是用来快速配置依赖jar包和管理项目中所使用的资源**。

聚合和继承的作用:

* 聚合用于快速构建项目，对项目进行管理
* 继承用于快速配置和管理子项目中所使用jar包的版本

聚合和继承的相同点:

* 聚合与继承的pom.xml文件打包方式均为pom，可以将两种关系制作到同一个pom文件中
* 聚合与继承均属于设计型模块，并无实际的模块内容

聚合和继承的不同点:

* 聚合是在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些
* 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己

### 多环境开发

* 父工程中定义多环境

  ```xml
  <profiles>
  	<profile>
      	<id>环境名称</id>
          <properties>
          	<key>value</key>
          </properties>
          <activation>
          	<activeByDefault>true</activeByDefault>
          </activation>
      </profile>
      ...
  </profiles>
  ```

* 使用多环境(构建过程)

  ```
  mvn 指令 -P 环境定义ID[环境定义中获取]
  ```

## SpringBoot

`SpringBoot` 是由Pivotal团队提供的全新框架，其设计目的是用来==简化==Spring应用的==初始搭建==以及==开发过程==。

原始 `Spring` 环境搭建和开发存在以下问题：

* 配置繁琐
* 依赖设置繁琐

`SpringBoot` 程序优点恰巧就是针对 `Spring` 的缺点

* 自动配置。这个是用来解决 `Spring` 程序配置繁琐的问题
* 起步依赖。这个是用来解决 `Spring` 程序依赖设置繁琐的问题
* 辅助功能（内置服务器,...）。我们在启动 `SpringBoot` 程序时既没有使用本地的 `tomcat` 也没有使用 `tomcat` 插件，而是使用 `SpringBoot` 内置的服务器。

<img src="SSM\image-20210911172200292.png" alt="image-20210911172200292" style="zoom:80%;" />

### 起步依赖

我们使用 `Spring Initializr`  方式创建的 `Maven` 工程的的 `pom.xml` 配置文件中自动生成了很多包含 `starter` 的依赖，如下图

<img src="SSM\image-20210918220338109.png" alt="image-20210918220338109" style="zoom: 80%;" />

从上面的文件中可以看到指定了一个父工程，我们进入到父工程，发现父工程中又指定了一个父工程，如下图所示

<img src="SSM\image-20210918220855024.png" alt="image-20210918220855024" style="zoom:80%;" />

再进入到该父工程中，在该工程中我们可以看到配置内容结构如下图所示

<img src="SSM\image-20210918221042947.png" alt="image-20210918221042947" style="zoom:80%;" />

> 上图中的 `properties` 标签中定义了各个技术软件依赖的版本，避免了我们在使用不同软件技术时考虑版本的兼容问题。
>
> `dependencyManagement` 标签是进行依赖版本锁定，但是并没有导入对应的依赖；如果我们工程需要那个依赖只需要引入依赖的 `groupid` 和 `artifactId` 不需要定义 `version`。

## 参考

黑马程序员SSM：https://www.bilibili.com/video/BV1Fi4y1S7ix/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=bf952648bf410c0b9b23bf213e3d24ba

JavaGuide：https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html

spring中BeanFactory和FactoryBean的区别：https://blog.csdn.net/dongyang2019/article/details/113725058

Nan-ying's blog：https://nan-ying.github.io/2023/07/10/Spring/#%E6%B3%A8%E8%A7%A3%E7%9A%84%E6%A6%82%E5%BF%B5

牛客高启盛同学资料：https://github.com/viego1999/JavaWxy