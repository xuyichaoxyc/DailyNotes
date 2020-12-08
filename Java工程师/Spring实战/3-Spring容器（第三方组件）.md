Spring容器职责：创建对象、装配对象、配置对象、管理生命周期

Spring自带的多个容器实现：

1. bean工厂：最简单的容器，提供基本的DI支持
2. 应用上下文：基于bean工厂构建，提供应用框架级别的服务

应用上下文类型：（自带，常用）

1. AnnotationConfigApplicationContext：从一个或多个基于Java 的配置类中加载Spring应用上下文
2. AnnotationConfigWebApplicationContext：从一个或多个基于Java 的配置类中加载 Spring Web应用上下文
3. ClassPathXmlApplicationContext：从类路径下的一个或多个XML配置文件中加载上下文定义，把应用上下文的定义文件作为类资源
4. FileSystemXmlApplicationContext：从文件系统下的一个或多个XML配置文件中加载上下文定义，把应用上下文的定义文件作为类资源
5. XmlWebApplicationContext：从Web应用下的一个或多个XML配置文件中加载上下文定义

bean的生命周期：（典型生命周期过程，不排除通过Spring提供的扩展点来自定义bean的创建过程）
	传统：new创建，不使用，垃圾自动回收

1. 实例化
2. 填充属性
3. 调用BeanNameAware的setBeanName方法
4. 调用BeanFactoryAware的setBeanFactory方法
5. 调用ApplicationContextAware的setApplicationContext方法
6. 调用BeanPostProcessor的预初始化方法
7. 调用InitializingBean的afterProperties方法
8. 调用自定义的初始化方法
9. 调用BeanPostProcessor的初始化后方法
10. 使用Bean
11. 容器关闭，调用DisposableBean的destroy方法
12. 调用自定义的销毁方法