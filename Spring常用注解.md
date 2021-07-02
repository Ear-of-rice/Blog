# 1)	用于注册bean对象注解

## 1.1 `@Component`

### **作用：**

```java
调用无参构造创建一个bean对象，并把对象存入spring的IOC容器，交由spring容器进行管理。相当于在xml中配置一个bean。
```

### **属性：**

```java
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写
```



## 1.2 `@Controller`

### 作用：

```java
作用上与@Component。一般用于表现层的注解。
```



### 属性：

```java
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```



## 1.3 `@Service`

### 作用：

```
作用上与@Component。一般用于业务层的注解。
```

### 属性：

```
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```



## 1.4 `@Repository`

### 作用：

```
作用上与@Component。一般用于持久层的注解。
```



### 属性：

```
value：指定bean的id。如果不指定value属性，默认bean的id是当前类的类名。首字母小写。
```



## 1.5 `@Bean`

### **作用：**

```
用于把当前方法的返回值作为bean对象存入spring的ioc容器中
```

### **属性：**

```
name：用于指定bean的id。当不写时，默认值是当前方法的名称。注意：当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中查找有没有可用的bean对象，查找的方式和Autowired注解的作用是一样的。
```

### **案例：**

```java
/**
 * 获取DataSource对象
 * @return
 */
@Bean(value = "dataSource")
public DataSource getDataSource() {
    try {
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        dataSource.setDriverClass(this.driver);
        dataSource.setJdbcUrl(this.url);
        dataSource.setUser(this.username);
        dataSource.setPassword(this.password);
        return dataSource;
    }catch (Exception exception) {
        throw new RuntimeException(exception);
    }
}
```

# 2）用于依赖注入的注解

## 2.1 `@Autowired`

### **作用：**

```
@Autowire和@Resource都是Spring支持的注解形式动态装配bean的方式。Autowire默认按照类型(byType)装配，如果想要按照名称(byName)装配，需结合@Qualifier注解使用。
```

### **属性：**

```
required：@Autowire注解默认情况下要求依赖对象必须存在。如果不存在，则在注入的时候会抛出异常。如果允许依赖对象为null，需设置required属性为false。
```

### **案例：**

```
@Autowire 
@Qualifier("userService") 
private UserService userService;
```



## 2.2 `@Qualifier`

### **作用：**

```
在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。它在给字段注入时不能独立使用，必须和 @Autowire一起使用；但是给方法参数注入时，可以独立使用。
```

### **属性：**

```
value：用于指定要注入的bean的id，其中，该属性可以省略不写。
```

### **案例：**

```
@Autowire
@Qualifier(value="userService") 
//@Qualifier("userService")     //value属性可以省略不写
private UserService userService;
```



## 2.3 `@Resource`

### **作用：**

```
@Autowire和@Resource都是Spring支持的注解形式动态装配bean的方式。@Resource默认按照名称(byName)装配，名称可以通过name属性指定。如果没有指定name，则注解在字段上时，默认取（name=字段名称）装配。如果注解在setter方法上时，默认取（name=属性名称）装配。
```

### **属性：**

```
name：用于指定要注入的bean的id
type：用于指定要注入的bean的type
```

### **装配顺序**

```
1.如果同时指定name和type属性，则找到唯一匹配的bean装配，未找到则抛异常；
2.如果指定name属性，则按照名称(byName)装配，未找到则抛异常；
3.如果指定type属性，则按照类型(byType)装配，未找到或者找到多个则抛异常；
4.既未指定name属性，又未指定type属性，则按照名称(byName)装配；如果未找到，则按照类型(byType)装配。
```

### **案例：**

```java
@Resource(name="userService")
//@Resource(type="userService")
//@Resource(name="userService", type="UserService")
private UserService userService;
```



## 2.4 `@Value`

### **作用：**

```
通过@Value可以将外部的值动态注入到Bean中，可以为基本类型数据和String类型数据的变量注入数据
```

### **案例：**

```java
// 1.基本类型数据和String类型数据的变量注入数据
@Value("tom") 
private String name;
@Value("18") 
private Integer age;


// 2.从properties配置文件中获取数据并设置到成员变量中
// 2.1jdbcConfig.properties配置文件定义如下
jdbc.driver \= com.mysql.jdbc.Driver  
jdbc.url \= jdbc:mysql://localhost:3306/eesy  
jdbc.username \= root  
jdbc.password \= root

// 2.2获取数据如下
@Value("${jdbc.driver}")  
private String driver;

@Value("${jdbc.url}")  
private String url;  
  
@Value("${jdbc.username}")  
private String username;  
  
@Value("${jdbc.password}")  
private String password;
```



# 3）用于改变bean作用范围的注解

## 3.1 `@Scope`

### **作用：**

```
指定bean的作用范围。
```

### **属性：**

```java
value：
    1）singleton：单例
    2）prototype：多例
    3）request： 
    4）session： 
    5）globalsession：
```

### **案例：**

```java
@Autowire
@Scope(value="prototype")
private UserService userService;
```



# 4）生命周期相关的注解

## 4.1 `@PostConstruct`

### **作用：**

```
指定初始化方法
```

### **案例：**

```java
@PostConstruct  
public void init() {  
    System.out.println("初始化方法执行");  
}
```



## 4.2 `@PreDestroy`

### **作用：**

```
指定销毁方法
```

### **案例：**

```java
@PreDestroy  
public void destroy() {  
    System.out.println("销毁方法执行");  
}
```



# 1.声明bean的注解

```
*@Component\**** 组件，没有明确的角色

**@Service** 在业务逻辑层使用（service层）

***\*@Repository\**** 在数据访问层使用（dao层）

**@Controller** 在展现层使用，控制器的声明（C）
```



# 2.注入bean的注解

```
@Autowired：由Spring提供

@Inject：由JSR-330提供

@Resource：由JSR-250提供

都可以注解在set方法和属性上，推荐注解在属性上（一目了然，少写代码）。
```



# 3.java配置类相关注解

```
@Configuration 声明当前类为配置类，相当于xml形式的Spring配置（类上）

@Bean 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上）

@Configuration 声明当前类为配置类，其中内部组合了@Component注解，表明这个类是一个bean（类上）

@ComponentScan 用于对Component进行扫描，相当于xml中的（类上）

@WishlyConfiguration 为@Configuration与@ComponentScan的组合注解，可以替代这两个注解
```



# 4.切面（AOP）相关注解

```
Spring支持AspectJ的注解式切面编程。

**@Aspect** 声明一个切面（类上） 
使用***\*@After、@Before、@Around\****定义建言（advice），可直接将拦截规则（切点）作为参数。

***\*@After\**** 在方法执行之后执行（方法上） 
***\*@Before\**** 在方法执行之前执行（方法上） 
***\*@Around\**** 在方法执行之前与之后执行（方法上）

**@PointCut** 声明切点 
在java配置类中使用@EnableAspectJAutoProxy注解开启Spring对AspectJ代理的支持（类上）
```



# 5.@Bean的属性支持

```
*@Scope\**** 设置Spring容器如何新建Bean实例（方法上，得有@Bean） 
其设置类型包括：

Singleton （单例,一个Spring容器中只有一个bean实例，默认模式）, 
Protetype （每次调用新建一个bean）, 
Request （web项目中，给每个http request新建一个bean）, 
Session （web项目中，给每个http session新建一个bean）, 
GlobalSession（给每一个 global http session新建一个Bean实例）

***\*@StepScope\**** 在Spring Batch中还有涉及

***\*@PostConstruct\**** 由JSR-250提供，在构造函数执行完之后执行，等价于xml配置文件中bean的initMethod

***\*@PreDestory\**** 由JSR-250提供，在Bean销毁之前执行，等价于xml配置文件中bean的destroyMethod
```



# 6.@Value注解

**@Value** 为属性注入值（属性上） 
支持如下方式的注入： 
》注入普通字符

```
@Value("Michael Jackson")



String name;
```

》注入操作系统属性

```
@Value("#{systemProperties['os.name']}")



String osName;
```

》注入表达式结果

```
@Value("#{ T(java.lang.Math).random() * 100 }") 



String randomNumber;
```

》注入其它bean属性

```
@Value("#{domeClass.name}")



String name;
```

》注入文件资源

```
@Value("classpath:com/hgs/hello/test.txt")



String Resource file;
```

》注入网站资源

```
@Value("http://www.cznovel.com")



Resource url;
```

》**注入配置文件**

```
@Value("${book.name}")



String bookName;
```

### 注入配置使用方法： 

```
① 编写配置文件（test.properties）

book.name=《三体》

② @PropertySource 加载配置文件(类上)

@PropertySource("classpath:com/hgs/hello/test/test.propertie")

③ 还需配置一个PropertySourcesPlaceholderConfigurer的bean。
```



# 7.环境切换

```
@Profile 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境。（类或方法上）

***\*@Conditional\**** Spring4中可以使用此注解定义条件话的bean，通过实现Condition接口，并重写matches方法，从而决定该bean是否被实例化。（方法上）
```



# 8.异步相关

```
@EnableAsync** 配置类中，通过此注解开启对异步任务的支持，叙事性AsyncConfigurer接口（类上）

**@Async** 在实际执行的bean方法使用该注解来申明其是一个异步任务（方法上或类上*所有的方法都将异步*，需要@EnableAsync开启异步任务）
```



# 9.定时任务相关

```
@EnableScheduling** 在配置类上使用，开启计划任务的支持（类上）

**@Scheduled** 来申明这是一个任务，包括cron,fixDelay,fixRate等类型（方法上，需先开启计划任务的支持）
```



# **10.@Enable\*注解说明**

这些注解主要用来开启对xxx的支持。 

```
@EnableAspectJAutoProxy** 开启对AspectJ自动代理的支持

**@EnableAsync** 开启异步方法的支持

**@EnableScheduling** 开启计划任务的支持

**@EnableWebMvc** 开启Web MVC的配置支持

**@EnableConfigurationProperties** 开启对@ConfigurationProperties注解配置Bean的支持

**@EnableJpaRepositories** 开启对SpringData JPA Repository的支持

**@EnableTransactionManagement** 开启注解式事务的支持

**@EnableTransactionManagement** 开启注解式事务的支持

**@EnableCaching** 开启注解式的缓存支持

11.测试相关注解

**@RunWith** 运行器，Spring中通常用于对JUnit的支持

**@ContextConfiguration** 用来加载配置ApplicationContext，其中classes属性用来加载配置类
```



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes={TestConfig.class})
public class KjtTest {
    private static Logger logger = LoggerFactory.getLogger("KjtTest");

    @Autowired
    Service service;
    @Test
    public void test() {
        
    }
}
```

 

# SpringMVC部分

```java
@EnableWebMvc** 在配置类中开启Web MVC的配置支持，如一些ViewResolver或者MessageConverter等，若无此句，重写WebMvcConfigurerAdapter方法（用于对SpringMVC的配置）。

***\*@Controller\**** 声明该类为SpringMVC中的Controller

***\*@RequestMapping\**** 用于映射Web请求，包括访问路径和参数（类或方法上）

**@ResponseBody** 支持将返回值放在response内，而不是一个页面，通常用户返回json数据（返回值旁或方法上）

**@RequestBody** 允许request的参数在request体中，而不是在直接连接在地址后面。（放在参数前）

**@PathVariable** 用于接收路径参数，比如@RequestMapping(“/hello/{name}”)申明的路径，将注解放在参数中前，即可获取该值，通常作为Restful的接口实现方法。

**@RestController** 该注解为一个组合注解，相当于@Controller和@ResponseBody的组合，注解在类上，意味着，该Controller的所有方法都默认加上了@ResponseBody。

**@ControllerAdvice** 通过该注解，我们可以将对于控制器的全局配置放置在同一个位置，注解了@Controller的类的方法可使用@ExceptionHandler、@InitBinder、@ModelAttribute注解到方法上， 
这对所有注解了 @RequestMapping的控制器内的方法有效。

**@ExceptionHandler** 用于全局处理控制器里的异常

**@InitBinder** 用来设置WebDataBinder，WebDataBinder用来自动绑定前台请求参数到Model中。

**@ModelAttribute** 本来的作用是绑定键值对到Model里，在@ControllerAdvice中是让全局的@RequestMapping都能获得在此处设置的键值对。
```

