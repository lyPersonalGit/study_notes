### I 注册组件



#### @Bean

作用： 该注解只能写在方法上，**表明把当前方法的返回值作为 bean 对象存入 Spring 容器中。**

**属性**： name：给当前 `@Bean` 注解方法创建的对象指定一个名称(即 bean 的 id）。 默认值是当前方法的名称

**细节**：**当我们使用注解配置方法时，如果方法有参数，Spring 框架会去容器中查找有没有相匹配的 bean 对象，查找方法和 `AutoWired` 一样。**

``` java
@Configuraion
public class AppConfig {
    @Bean("user")
    public User initUser() {
        User user = new User();
        user.setId(1L);
        user.setUserName("user_name");
        user.setNote("note");
        return user;
    }
}
```



#### @Component,@Controller,@Service,@Repository

作用：用于标明某个类被扫描进Spring容器中并装配，其中“user”则是作为Bean的名称。

**属性**： value：给当前`@Component`注解的类创建出来的对象指定一个名称。

**细节**：`@Component`是一个类的通用注解，而`@Controller`,`@Service`,`@Repository`用于标识特定用法的类，分别为提供接口的控制器类，实现业务业务逻辑的类，直接访问数据库的数据访问类。其源码都实现了`@Component`注解。

``` java
@Component("user")
public class User {
    @Value("1")
    private Long id;
    @Value("user_name")
    private String userName;
    @Value("note")
    private String note;
}
```



#### @Configuration

**作用**： **用于指定当前类是一个 Spring 配置类（配置文件），当创建容器时会从该类上加载注解，配置类里使用@Bean标注在方法上给容器注册组件**。

**属性**： value:用于指定配置类的字节码

**细节**：当配置类作为 `AnnotationConfigApplicationContext` 对象创建的参数时，该配置类上的 `@Configuration` 注解可以不写，其源码实现了`@Component`注解，使用该注解的类其本身也是一个组件。

``` java
@Configuraion
public class AppConfig {
    @Bean("user")
    public User initUser() {
        User user = new User();
        user.setId(1L);
        user.setUserName("user_name");
        user.setNote("note");
        return user;
    }
}
```



### II 自动扫描



#### @ComponentScan

作用： **用于指定 Spring 在初始化容器时要扫描的包**。

属性： `basePackages / value`：用于指定要扫描的包

细节：springboot的启动类注解`@SpringBootApplication`中实现了`@ComponentScan`注解，其默认的扫描范围是该启动类所在包下的所有包。将当前所在包下所有的bean装配到springboot的ioc容器中。

``` java
@Configuration 
@ComponentScan("com.smallbeef") 
public class JDBCConfiguration { 

}
```



### III 依赖注入



#### @Autowired

**作用**： 从容器中查找已在容器中注册的组件，并自动按照类型注入。

**出现位置**：变量和方法上都可以

当使用注解注入属性时，set 方法可以省略。它只能注入其他 bean 类型。

在 Spring 容器查找，找到了注入成功。找不到 就报错。

**示例代码：**

``` java
@Autowired
private IUserService userServiceImpl;
```



#### @Primary和@Qualifier

**作用**： 在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。

**它在给成员变量注入时不能独立使用，必须和 `@Autowire` 一起使用；但是给方法参数注入时，可以独立使用**

**属性**： value：指定 bean 的 id。

**示例代码**

``` java
@Autowired
@Qualifier("UserServiceImpl")
private IUserService userServiceImpl;
```



#### @Resource

**作用**： 直接按照 Bean 的 id 注入。**可以独立使用**（相当于Autowired + Qualifier）。它也只能注入其他 bean 类型。

**属性**： **name**：指定 bean 的 id (可不写)。

**示例代码：**

``` java
@Resource("cat")
private Animal cat;

//等效于
@Autowired
@Qualifier("cat")
private Animal cat;
```



#### @Value

**作用**： **注入基本数据类型和 String 类型的数据** 。和 依赖注入 Xml 配置中的value属性作用相同

**属性**： value：用于指定值。它可以使用 Spring 中 `SpEL`（也就是spring中的EL表达式, `${表达式}`）

**示例代码：**

``` java
@Value("${name}")
private String name;
```



### IV 配置绑定



#### @ConfigurationProperties

**作用：将application.yml或application.properties配置文件中的属性与注解的类进行绑定。默认从全局配置文件中获取值。**

**属性**：prefix：与配置文件中哪个属性进行一一映射

**yaml文件：**

``` yaml
person:
    lastName: hello
    age: 18
    boss: false
    birth: 2017/12/12
    maps: {k1: v1,k2: 12}
    lists:
      - lisi
      - zhaoliu
    dog:
      name: 小狗
      age: 12
```

**properties文件：**

``` properties
person.last-name=张三
person.age=11
person.birth=2017/12/15
person.boss=false
person.maps.k1=v1
person.maps.k2=14
person.lists=a,b,c
person.dog.name=dog
person.dog.age=15
```

**注解类：**

``` java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {

    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    
    // 有参无参构造、get、set方法、toString()方法
}
```

**使用该注解需要在pom中引用：**

``` xml
<!--configuration 装配-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```



#### @PropertySource

作用：使用`@PropertySource`注解可以从自定义的文件中获取值

属性：value：指定自定义的文件名称

**细节：`@PropertySource`注解只能匹配的自定义的properties文件，不能匹配自定义的yaml文件。`@PropertySource`注解只用于标注配置文件的来源，需要配合`@ConfigurationProperties`或`@Value`注解来使用。**

``` java
@Component
@ConfigurationProperties(prefix = "admin")
@PropertySource(value = {"classpath:admin.properties"})
public class AdminAccount {

    private String account;
    private String password;
    public String getAccount() {
        return account;
    }
    
    // 有参无参构造、get、set方法、toString()方法
}
```



#### @Value

**作用**： **注入基本数据类型和 String 类型的数据** 。和 依赖注入 Xml 配置中的value属性作用相同

**属性**： value：用于指定值。它可以使用 Spring 中 `SpEL`（也就是spring中的EL表达式, `${表达式}`）

**示例代码：**

``` java
@Value("${name}")
private String name;
```



#### @ConfigurationProperties和@Value的区别

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| **复杂类型封装**     | **支持**                 | **不支持** |



### V Spring AOP



#### AOP术语和流程

- **连接点（Joinpoint)** 程序执行的某个特定位置，如某个方法调用前，调用后，方法抛出异常后，这些代码中的特定点称为连接点。简单来说，就是在哪加入你的逻辑增强

- **通知（Advice）** 增强是织入到目标类连接点上的一段程序代码。在 Spring 中还可为增强代码指定方位信息：

  - **前置通知(before)**:在执行业务代码前做些操作，比如获取连接对象
  - **后置通知(after)**:在执行业务代码后做些操作，无论是否发生异常，它都会执行，比如关闭连接对象
  - **异常通知（afterThrowing）**:在执行业务代码后出现异常，需要做的操作，比如回滚事务
  - **返回通知(afterReturning)**,在执行业务代码后无异常，会执行的操作
  - **环绕通知(around)**，这个目前跟我们谈论的事务没有对应的操作，所以暂时不谈

- **切点/切入点（PointCut）** 就是带有通知的连接点，在程序中主要体现为书写切入点表达式

- **目标对象（Target）** 需要被加强的业务对象

- **织入（Weaving）** 织入就是将增强添加到对目标类具体连接点上的过程。

  织入是一个形象的说法，具体来说，就是生成代理对象并将切面内容融入到业务流程的过程。

- **切面（Aspect）** 切面由切点和增强组成，它既包括了横切逻辑的定义，也包括了连接点的定义，SpringAOP 就是将切面所定义的横切逻辑织入到切面所制定的连接点中。**通常来说切面是一个类（切面类），即对该类的某个方法进行增强**

#### 确定连接点



#### 定义切点



#### 通知

**示例代码**

``` java
@Component
@Aspect
public class MyAspect {
    @PointCut("* service.*")
    public void pointCut() {}
    
    @Before("pointCut()")
    public void before() {
        System.out.println("before......");
    }
    
    @After("pointCut()")
    public void after() {
        System.out.println("after......");
    }
    
    @AfterReturning("pointCut()")
    public void afterReturning() {
        System.out.println("afterReturning......");
    }
    
    @AfterThrowing("pointCut()")
    public void afterThrowing() {
        System.out.println("afterThrowing......");
    }
}
```

**打印结果**

``` 
before......
id=1 username=user_name_1 note=2323
after......
afterReturning......
```

