[TOC]



## Spring 作用

简化 Java EE应用开发，包括web应用，以POJO为基础的编程模型



## Spring 好处

### 轻量级

### 控制反转（IOC）：

实现松散耦合

### 依赖注入（DI）

是IOC的一个方面，有多种解释

不用创建对象，只需要描述它如何被创建。不在代码里直接组装你的组件和服务，而在配置文件里描述那些组件需要哪些服务，之后一个容器（IOC）容器负责把他们组装起来。

两种依赖注入方式：

+ **构造器依赖注入**，
+ **Setter方法注入**：容器通过调用无参构造器或无参static工厂方法实例化bean之后，调用该bean的setter方法，实现依赖注入

其中，构造器参数实现强制依赖，setter方法实现可选依赖

### 面向切面编程（AOP）

### 容器：

Spring 包含并管理应用中对象的生命周期和配置

### MVC框架：

Spring 的 WEB 框架是个精心设计的框架，是Web框架的一个很好的替代品

### 事务管理：

提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）

### 异常处理：

提供方便的API把具体技术相关的异常（比如JDBC，Hibernate or JDO抛出的）转化为一致的unchecked异常



## Spring 模块组成：

+ Core
+ Bean
+ Context
+ Expression Language 
+ JDBC
+ ORM
+ OXM
+ Java Messaging Service（JMS）
+ Transaction 
+ Web
+ Web-Servlet
+ Web-Structs
+ Web-Portlet

## 核心容器（应用上下文）模块

基本的Spring 模块，提供 Spring 框架的基础功能，BeanFactory 是任何以 Spring 为基础的应用的核心。Spring框架建立在此模块之上，使 Spring 成为一个容器



Spring IOC 负责创建对象，管理对象（通过依赖注入），装配对象，配置对象，并且管理这些对象的生命周期



## BeanFactory & ApplicationContext

Bean 工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从真正的应用代码中分离。

最常用的 Bean Factory 实现是 XmlBeanFactory

### XmlBeanFactory

```java
org.springframework.beans.factory.xml.XmlBeanFactory
```

根据 XML 文件中的定义加载 beans

该容器从 XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用

### ApplicationContext的实现：

```java
FileSystemXmlApplicationContext
ClassPathXmlApplicationContext
WebXmlApplicationContext
```

### 两者区别

## Spring 配置方式

### XML 配置文件

### 基于注解的配置

+ 依赖于通过字节码元数据装配组件
+ 在相应的类、方法或属性上使用注解的方式，直接组件类中进行配置

#### 开启注解装配

默认不开启，在 Spring 配置文件中配置：

```xml
<context:annotation-config/>
```

#### 注解解释

#### @Required

bean 的属性必须在配置的时候设置，通过一个 bean 定义的显示的属性值或通过自动装配

若该注解的 bean  属性未被设置， BeanInitializationException

#### @Autowired

修饰 setter方法、构造器、属性或者具有任意名称和/或多个参数的 PN 方法

#### @Qualifier

将其和@Autowired 结合使用消除多个同类型bean，却只有一个需要自动装配时的混淆，指定需要装配的确切的bean

### 基于 Java 的配置

+ 可以在少量的 Java 注解的帮助下，进行大部分的 Spring 配置，而非通过 XML 文件。

## Spring java 集合的注入

## Spring Bean 的生命周期

## Spring Bean 的装配

### 显示装配

<constructor-arg>

<property>

### 自动装配

no

byName

byType

constructor

autodetect

### 自动装配的局限性

重写

基本数据类型无法装配，String字符串、类

模糊特性