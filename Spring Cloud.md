[TOC]



## Spring Cloud 整体技术--子项目

### 微服务构建：Spring Boot

### 微服务治理：Spring Cloud Eureka

### 客户端负载均衡：Spring Cloud Ribbon

### 服务容错保护：Spring Cloud Hystrix

### 声明式服务调用：Spring Cloud Feign

### API 网关服务：Spring Cloud Zuul

### 分布式配置中心：Spring Cloud Config、Apollo

### 消息总线：Spring Cloud Bus

### 消息驱动的微服务：Spring Cloud Stream

### 分布式服务跟踪：Spring  Cloud Sleuth



## Spring Boot

### 构建 Maven 项目

Spring Initializer 工具产生基础项目（Spring Boot 项目）

添加相关依赖（web 依赖等）

### Spring Boot 工程结构解析

+ src/main/java：主程序入口
+ src/main/resources：配置目录，用以存放应用的一些配置信息，如应用名、服务端口、数据库链接等，引入Web模块，还会产生 static （存放静态资源，如图片、CSS、JavaScript等）、template（存放Web页面的模板文件）
+ src/test/：单元测试目录

### Maven 配置分析

+ groupId、artifactId，项目标识
+ <packaging></packaging>打包方式 ：pom、war、jar
+ 父项目 parent 配置
  + spring-boot-starter-parent：定义了Spring Boot版本的基础依赖以及一些默认配置内容，比如配置文件application.properties的位置等
+ 项目依赖 dependencies、dependence
  + spring-boot-starter-web：全栈 Web 开发模块，包含嵌入式 Tomcat、Spring MVC
  + spring-boot-starter-test：通用测试模块，包含 JUnit、Hamcrest、Mockito
  + 上面引用的 web 和 test 模块，在 Spring Boot 生态中被称为 Starter POMs。Starter POMs 是一系列轻便的依赖包，是一套一站式的 Spring 相关技术的解决方案。开发者在使用和整合模块时，不必再去搜寻样例代码中的依赖配置来复制使用，只需要引入对应的模块包即可。
  + 开发 Web 应用，引入 spring-boot-starter-web，需要访问数据库，引入 spring-boot-starter-jdbc 或更好用的 spring-boot-starter-data-jpa。
+ build 部分，项目构建
  + <plugins><plugin>
  + spring-boot-maven-plugin
  + 方便启停应用，不用每次寻找主类，或者打包 jar 包来运行微服务，mvn spring-boot：run命令就可以快速启动 Spring Boot 应用。

### RESTful API

在 Spring Boot 中创建一个 RESTful API 的实现代码同 Spring MVC 应用一样，不需像Spring MVC 那样先做很多配置，直接编写 Controller 内容即可

```java
@RestController
public class HelloController{
    @RequesMapping("/hello")
    public String index(){
        return "Hello, World";
    }
}
```

### 启动Spring Boot 应用

+ 直接通过运行拥有 main 函数的类来启动
+ 在 Maven配置中，spring-boot 插件，使用其来启动，比如执行 mvn spring-boot:run，或这直接单机 IDE 中对 Maven 插件的工具
  + Maven->项目->Plugin->spring-boot
+ 在服务器上部署运行时，通常先使用 mvn install 将应用打包成 jar 包，再通过 java -jar xxx.jar

### 编写单元测试

随手写配套单元测试，在微服务架构中尤为重要

```java
// src/test/测试入口

// 引入 Spring 对 JUnit4 的支持
@RunWith(SpringJUnit4ClassRunner.class)
// 指定 Spring Boot 的启动类
@SpringApplicationConfiguration(classes = HelloApplication.class)
// 开启 Web 应用的配置，用于模拟 ServletContext
@WebAppConfiguration
public class HelloApplicationTest{
    // MockMvc 对象：用于模拟调用 Controller 的接口发起请求
    private MockMvc mvc;
    
    // JUnit 中定义在测试用例@Test 内容执行前预加载的聂荣，
    @Before
    public void setUp() throws Exception{
        mvc = MockMvcBuilders.standaloneSetup(new HelloController()).build();
    }
    
    @Test
    public void hello() throws Exception{
        mvc.perform(MockMvcRequestBuilders.get("/hello").accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOK())
            .andExpect(content().string(equalTo("Hello World")));
    }
}
```

+ JUnit
+ spring-boot-starter-test
+ Mock 测试

### 配置文件

配置目录：/src/main/resources

配置文件：/src/main/resources/application.properties

关于 Spring Boot 应用的配置内容都可以集中在该文件，根据引入的不同 Starter 模块，可以在这里定义容器端口号、数据库连接信息、日志级别等各种配置信息。

Web模块的服务端口号：server.port = 8888

spring.application.name = hello

+ properties
+ yaml



