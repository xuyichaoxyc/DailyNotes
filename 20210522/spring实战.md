[TOC]

# 装配 Bean

## 1. 声明 bean，@Component

```java
public interface CompactDisc{
    void play();
}


@Component
// Component 注解表明该类会作为组件类，Spring为该类创建bean
public class SgtPeppers implements CompactDisc{
    private String title = "Sgt. Pepper's Lonely Hearts Club Band";
    private String artist = "The Beatles";
    
    public void play(){
        System.out.println("playing " + title )
    }
}

/**
	@Component——默认类名首字母小写
	@Component("beanName")

	Java依赖注入规范（Java Dependency Injection）
	@Named（"beanName"）


	@Component和@Named
	Spring 支持将 @Named 作为 @Component的替代方案，两者之间有一些细微的差异，在大多数场景中，它们是可以相互替换的
	
	
	Spring Web中：
	@Component
		@Service
		@Repository
		@Controller
*/
```

## 2. 组件扫描，@ComponentScan

组件扫描默认不启用

```java
@Configuration
@ComponentScan
public class CDPlayerConfig{
    
}
/*
	@Configuration 声明该类为一个配置类
	被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。
	
	@ComponentScan 启用组件扫描，默认扫描与配置类相同的包（该包及包下的所有子包）
	该类通过Java代码定义了 Spring的装配规则
	
	扫描不同包：
	@ComponentScan("xxx")
	@ComponentScan(basePackages="xxx")
	@ComponentScan(basePackages={"xxx", "yyy"})
	@ComponentScan(basePackagesClasses={xxx.class, yyy.class})
*/
```

```xml
通过 xml 启用组件扫描

<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:Context="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    
    <context:component-scan base-package="soundsystem"/>

</beans>
       
```

## 3. 自动装配(注解)，@Autowired

```java
@Component
public class CDPlayer implements MediaPlayer{
    private CompackDisc cd;
    
    // Autowired 注解使用在构造器上
    @Autowired
    public CDPlayer(CompactDisc cd){
        this.cd = cd;
    }
    
    public void play(){
        cd.play();
    }
}


// Autowired 注解使用在 属性的setter方法上
@Autowired
public void setCompactDisc(CompactDisk cd){
    this.cd = cd;
}

// Spring 初始化 bean 之后，尽可能去满足bean的依赖，
// 可通过带有 @Autowired 注解的方法进行声明
// @Autowired 注解可用在类的任何方法上
@Autowired
public void insertDisc(CompactDisc cd){
    this.cd = cd;
}

// 不管是构造器、Setter 方法、还是其他的方法，Spring都会尝试满足方法参数上所声明的依赖
// 只有一个bean 满足匹配，进行装配
// 0个，Spring抛出异常，@AutoWired(required=false)	可以避免抛出装配异常，该bean处于未装配状态，但是可能造成后续 NullPointerException。
// 多个，Spring抛出异常，表明没有明确指定选择哪个bean进行装配


/*
@Autowired	Spring特有注解

可采用 Java 依赖注入规范注解	@Inject 进行替代

*/
@Named
public class CDPlayer{
    
    @Inject
    public CDPlayer(CompactDisc cd){
        this.cd = cd;
    }
    
}



// @Autowired 注解类私有属性

public class CDPlayerTest{
    @Autowired
    private MediaPlayer player;
    
    @Autowired
    private CompactDisc cd;
}
```

## 4. Java代码装配，@Configuration、@Bean

```java
// 创建配置类
@Configuration
public class CDPlayerConfig{
    
}

/*
	@Configuration 表明该类为一个配置类，包含在Spring应用上下文如何创建 bean 的细节
	
*/

@Configuration
public class CDPlayerConfig{
    @Bean
    public CompactDisc sgtPeppers(){
        return new SgtPeppers();
    }
}
/*
	@Bean 注解方法，返回的对象注册为 Spring应用上下文中的bean，方法体中包含了最终产生 bean 实例的逻辑
*/

@Configuration
public class CDPlayerConfig{
    @Bean(name="xxxx")
    public CompactDisc sgtPeppers(){
        return new SgtPeppers();
    }
}

@Configuration
public class CDPlayerConfig{
    @Bean
    public CompactDisc randomBeatlesCD(){
        int choice = (int) Math.floor(Math.random() * 4);
        if(choice == 0){
            return new SgtPeppers();
        }
        else if(choice == 1){
            return new WhiteAlbum();
        }
        else if(choice == 2){
            return new HardDaysNight();
        }
        else{
            return new Revolver();
        }
    }
}



@Bean
public CDPlayer cdPlayer(){
    return new CDPlayer(sgtPeppers());
}

@Bean 
public CDPlayer cdPlayer(CompactDisc compactDisc){
    return new CDPlayer(compactDisc);
}


@Bean
public CDPlayer cdPlayer(CompactDisc compactDisc){
    CDPlayer cdPlayer = new CDPlayer(compactDisc);
    cdPlayer.setCompactDisc(compactDisc);
    return cdPlayer;
}

/*
	带有 @Bean 注解的方法可以采用任何必要的Java功能来产生Bean示例。
	构造器和Setter方法只是 @Bean方法的两个简单样例。这里存在的可能性仅仅受到Java语言的限制。
*/
```



## 5. XML装配

```xml
<!-- XML 配置规范 -->
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:c="http://www.springframework.org/schema/c"	<!-- 声明 c 命名空间 -->
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:Context="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    
    <!-- configuration details go here -->
    <bean class="xxxx" />
    
    <bean id="beanName" class="beanClassName" />
    
<!-- =================================================== -->
    <bean id="beanName" class="beanClassName">
    	<constructor-arg ref="beanName-id" />
    </bean>

	<!-- =================================== -->
	<bean id="beanName" class="beanClassName">
    	<c:构造器参数名-ref="beanName-id" />
    </bean>
	<bean id="beanName" class="beanClassName">
        <!-- 构造器参数列表位置信息 -->
    	<c:_0-ref="beanName-id" />
    </bean>
	<bean id="beanName" class="beanClassName">
        <!-- 构造器只有一个参数 -->
    	<c:_-ref="beanName-id" />
    </bean>
	<!-- =================================== -->

	<bean id="beanName" class="beanClassName">
    	<constructor-arg value="xxx" />
    </bean>
	<bean id="beanName" class="beanClassName">
    	<c:_参数名="xxx" />
    </bean>
	
	<bean id="beanName" class="beanClassName">
    	<c:_0="xxx" />
        <c:_1="xxx" />
    </bean>

	<bean id="beanName" class="beanClassName">
    	<c:_="xxx" />
    </bean>
<!-- =================================================== -->


</beans>
```



## 5. 导入和混合配置，@Import、@ImportResource

```java
@Configuration
public class CDPlayerConfig{
    @Bean 
    public CompactDisc compactDisc(){
        
    }
    
    @Bean 
    public CDPlayer cdPlayer(CompactDisc compactDisc){
        return new CDPlayer(compactDisc);
    }
}


// 将上述配置类进行分解
@Configuration
public class CDConfig{
    @Bean 
    public CompactDisc compactDisc(){
        
    }
}

@Configuration
@Import(CDConfig.class)
public class CDPlayerConfig{
    @Bean 
    public CDPlayer cdPlayer(CompactDisc compactDisc){
        return new CDPlayer(compactDisc);
    }
}


// 创建一个更高级别的配置类，在这个类中使用Import将两个配置类组合在一起
@Configuration
@Import({CDConfig.class, CDPlayerConfig.class})
public class SoundSystemConfig{
    
}


// JavaConfig 和 XML 配置混合
@Configuration
@Import(CDPlayerConfig.class)
@ImportResource("classpath:cd-config.xml")
public class SoundSystemConfig{
    
}


// XML配置中引入xml配置 <import resource="xxx.xml">

// XML配置中引入JavaConfig <bean class="xxx.xxConfig">

// 根配置，根配置中启用组件扫描
	<context:component-scan>
    @ComponentScan
```



## 6. 总结

Spring 的核心是 容器，

容器负责：管理应用中组件的生命周期，它会创建这些组件并保证他们的依赖得到满足

装配 bean 的三种方式：自动化配置、基于Java的显示配置、基于XML的显示配置，均描述了 Spring应用中的组件及这些组件间的关系

尽可能使用自动化配置，避免显示配置带来的维护成本

如果确实需要显示配置，优先基于Java的显示配置，比基于XML的显式配置更强大，类型安全，且易重构



# 高级装配

## 1. Spring profile，@Profile，spring.profiles.active,Spring.profiles.default

开发环境、QA环境、生产环境，不同环境下，某些配置需要发生变化，数据库配置、加密算法以及与外部系统的集成

解决方法：单独的配置类（或XML文件），需要重新编译

```java
@Configuration
@Profile("dev")

// 声明该配置类（bean）属于dev profile
// @Profile 注解应用在了类级别上，告诉Spring这个配置类中的bean只有在 dev profile激活时才会创建，如果 dev profile 没有激活时，那么带有 @Bean 注解的方法都会被忽略掉


@Configuration
@Profile("prod")


@Bean
@Profile("dev")

@Bean
@Profile("prod")
```

xml 配置 profile

```xml
<beans 
       
       
       profile="dev">
    
</beans>

<!-- beans 嵌套定义 beans,将多个profile放置同一个xml中 --> 
```

激活profile

```yaml
spring.profiles.active
spring.profiles.default

如果设置了 spring.profile.active 属性，那么其值将会用来确定哪个profile是激活的。若未设置，
查找spring.profiles.default的值

二者均未设置，那就没有激活的profile，因此只会创建那些没有定义在profile中的bean


这两个属性的设置方式：
DispatcherServlet 的初始化参数
Web应用的上下文参数
JNDI条目
环境变量
JVM的系统属性
集成测试类上，@ActiveProfiles注解设置

```



## 2. 条件化的 bean 声明，@Conditional

判断某些条件是否满足，从而决定 bean 的创建与否

如：应用的类路径下是否包含特定的库

​		某个特定的 bean 是否声明

​		某个特定的环境变量是否设置



通过 @Conditional 注解完成条件化的 bean 声明，和 @Bean 一起使用



```java
@Bean
@Conditional(MagicExistsCondition.class)
public MagicBean magicBean(){
    return new MagicBean();
}


public interface Condition{
    boolean matches(ConditionContext ctxt,
                   AnnotatedTypeMetadata metadata);
}

public class MagicExistsCondition implements Condition{
    public boolean matches(ConditionContext ctxt,
                   AnnotatedTypeMetadata metadata){
        Environment env = context.getEnvironment();
        return env.containsProperty("magic");
    }
}


// 两个接口 ConditionContext、AnnotatedTypeMetadata
/*
	ConditionContext:
*/
public interface ConditionContext{
    // getRegistry()——检查Bean定义
    BeanDefinitionRegistry getRegistry();
    // getBeanFactory()——检查Bean是否存在，甚至探查Bean的属性
    ConfigurableListableBeanFactory getBeanFactory();
    // getEnvironment——检查环境变量是否存在以及它的值是什么
    Environment getEnvironment();
    // getResourceLoader()——读取并探查所加载的资源
    ResourceLoader getResourceLoader();
    // getClassLoader()——加载并检查类是否存在
    ClassLoader getClassLoader();
}

/*
	AnnotatedTypeMetadata
*/
public interface AnnotatedTypeMetadata{
    // isAnnotated()——判断带有@Bean注解的方法是不是还有其他特定的注解
    boolean isAnnotated(String annotationType);
    Map<String, Object> getAnnotationAttributes(String annotationType);
    Map<String, Object> getAnnotationAttributes(String annotationType, boolean classValuesAsString);
    MultiValueMap<String, Object> getAllAnnotationAttributes(String AnnotationType);
    MultiValueMap<String, Object> getAllAnnotationAttributes(String AnnotationType, boolean classValuesAsString);
}


/*
	@Qualifier
*/
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@Documented
@Conditional(ProfileCondition.class)
public @interface Qualifer{
    String value();
}

class ProfileCondition implements Condition{
    public boolean maches(CondtionContext context, AnnotatedTypeMetadata metadata){
        if(context.getEnvironment() != null){
            MultiValuesMap<String, Object> attrs = 
                metadata.getAllAnnotationAttributes(Profile.class.getName());
            if(attrs != null){
                for(Object value : attrs.get("value")){
                    if(context.getEnvironment().acceptsProfiles((String[]) value)){
                        return true;
                    }
                }
                return false;
            }
        }
        return true;
    }
}
```



## 3. 自动装配与歧义性，@Primary、@Qualifiler

NoUniqueBeanDefinitionException

+ 可选 bean 中的某个设为首选（primary）
+ 使用限定符（qualifier）来帮助Spring缩小范围，最后只有一个bean



```java
@Component
@Primary
public class IceCream implements Dessert{
    
}

@Bean
@Primary
public Dessert iceCream(){
    return new IceCream();
}

<bean id = "iceCream" class="com.desserteater.IceCream"
    primary="true" />
    
    
    
    
    
@Autowired
@Qualifier("iceCream")
public void setDessert(Dessert dessert){
    this.dessert = dessert;
}
```





## 4. bean的作用域，@Scope

+ 单例（Singleton）：在每个应用中，只创建 bean 的一个实例
+ 原型（prototype）：注入、获取，创建新的实例
+ 会话（Session）：Web应用中，每个会话创建一个新的实例
+ 请求（Request）

@Scope("prototype")

@Scope("ConfigurableBeanFactory.SCOPE_PROTOTYPE")



<bean scope="prototype" />





### 运行时值注入：

+ 属性占位符
+ Spring表达式语言（SpEL）

去除硬编码的僵硬

```java
// 注入外部的值
@Configuration
@PropertySource("xxx/xx.properties")	// 声明属性源
public class ExpressiveConfig{
    @Autowired
    Environment env;
    
    @Bean
    public BlankDisc disc(){
        // 检索属性值
        return new BlankDisc(
        	env.getProperty("disc.title"),
            env.getProperty("disc.artist")
        );
    }
}

XML文件中
<bean ...
    c:_title="${disc.title}"
    c:_artist="${disc.artist}" />
        
        
在无配置类及XML文件的方式下，自动配置采用@Value注解获取占位符值
public BlankDisc(@Value("${disc.title}") String title,
                 @Value("${disc.artist}") String artist){
        this.title = title;
        this.artist = artist;
    }

为了使用占位符，必须配置
PropertyPlaceholderConfigure bean
PropertySourcesPlaceholderConfigure bean——推荐
    
    
<context:property-placeholder />
```



## 5. Spring表达式语言（SpEL）

## 6. 总结