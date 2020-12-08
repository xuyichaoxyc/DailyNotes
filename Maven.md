[TOC]

Apache下一个纯Java开发的开源项目，Maven利用一个中央信息片段能管理一个项目的构建、报告和文档等步骤

一个项目管理工具，对Java项目进行构建、依赖管理

C#、Ruby、Scala和其他语言编写的项目

### 一、Maven功能

+ **构建**
+ 文档生成
+ 报告
+ **依赖**
+ SCMs
+ 发布
+ 分发
+ 邮件列表

### 二、约定配置

使用一个共同的标准目录结构，使用约定优于配置的原则

#### Maven目录结构

| 目录                                | 目的                                                         |
| :---------------------------------- | :----------------------------------------------------------- |
| ${basedir}                          | 存放pom.xml和所有的子目录                                    |
| ${basedir}/src/main/java            | 项目的java源代码                                             |
| ${basedir}/src/main/resources       | 项目的资源，比如说property文件，springmvc.xml                |
| ${basedir}/src/test/java            | 项目的测试类，比如说Junit代码                                |
| ${basedir}/src/test/resources       | 测试用的资源                                                 |
| ${basedir}/src/main/webapp/WEB- INF | web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面 |
| ${basedir}/target                   | 打包输出目录                                                 |
| ${basedir}/target/classes           | 编译输出目录                                                 |
| ${basedir}/target/test-classes      | 测试编译输出目录                                             |
| Test.java                           | Maven只会自动运行符合该命名规则的测试类                      |
| ~/.m2/repository                    | Maven默认的本地仓库目录位置                                  |
| pom.xml、LICENSE.txt、README.txt    |                                                              |



#### Web目录结构

– 应用程序根目录
– |-- WEB-INF目录：必须目录
– |-- web.xml：Web应用部署描述文件，必须目录
– |-- classes目录：存放字节码文件
– |-- lib目录：存放第三方类库文件
– |-- TLD文件：标签库描述文件
– |-- 其他静态文件：HTML、CSS、JavaScript、图片等

| **目录**         | **存放内容**                                                 |
| ---------------- | ------------------------------------------------------------ |
| css              | 存放.css格式文件（可再分目录）                               |
| skins            | 存放皮肤文件（按主题划分的framework的位图）                  |
| images           | 存放图片，按产品、功能模块划分子目录                         |
| js               | JavaScript文件（对象、函数库）                               |
| include          | 存放被包含的JS文件片段【注：JSP文件互相不要包含，通过模板/组件/标签库/BEAN实现重用】 |
| resources        | 存放JSF组件、相关资源等                                      |
| templates        | 模板文件存放地，按类别划分子目录                             |
| pages            | 网页目录（静态和动态网页，除index.jsp），按产品、功能模块划分子目录 |
| webapp下其他目录 | 解释为模块名，认为其中全部为网页，可再分子目录               |
| META-INF         | 存放清单文件、services等配置信息                             |
| WEB-INF          | 网站配置文件目录，存放WEB.XML等配置信息                      |
| WEB-INF/classes  | 未打包的项目编译代码，禁止手工修改。                         |
| WEB-INF/conf     | 存放struts,spring,hibernate,JSF等的配置文件                  |
| WEB-INF/lib      | 存放第三方JAR包，使用MAVEN构建时此目录禁止手动放入文件！     |
| WEB-INF/pages    | 高安全性的网页目录，如登录信息维护等                         |
| WEB-INF/tld      | JSP标签库定义文件存放目录                                    |

### 三、Maven环境配置

#### 1. JDK安装

| 项目     | 要求                                                         |
| :------- | :----------------------------------------------------------- |
| JDK      | Maven 3.3 要求 JDK 1.7 或以上<br />Maven 3.2 要求 JDK 1.6 或以上 <br />Maven 3.0/3.1 要求 JDK 1.5 或以上 |
| 内存     | 没有最低要求                                                 |
| 磁盘     | Maven 自身安装需要大约 10 MB 空间。除此之外，额外的磁盘空间将用于你的本地 Maven 仓库。你本地仓库的大小取决于使用情况，但预期至少 500 MB |
| 操作系统 | 没有最低要求                                                 |

#### 2. Maven 下载

下载地址：http://maven.apache.org/download.cgi

windows——zip

linux、mac——tar.gz

解压，

#### 3. 设置 Maven 环境变量

MAVEN_HOME:

E:\Maven\apache-maven-3.3.9

Path:

%MAVEN_HOME%\bin



### 四、POM

POM（Project Object Model，项目对象模型）是Maven工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构架，声明项目依赖，等等。