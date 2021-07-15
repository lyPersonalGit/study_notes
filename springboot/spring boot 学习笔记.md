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

**作用**： **用于指定当前类是一个 Spring 配置类，当创建容器时会从该类上加载注解**。

**属性**： value:用于指定配置类的字节码

**细节**：当配置类作为 `AnnotationConfigApplicationContext` 对象创建的参数时，该配置类上的 `@Configuration` 注解可以不写

### II 自动扫描

#### @ComponentScan



### III 依赖注入

#### @Autowired

**作用**： 从容器中查找已在容器中注册的组件，并自动按照类型注入。

**出现位置**：变量和方法上都可以

当使用注解注入属性时，set 方法可以省略。它只能注入其他 bean 类型。

在 Spring 容器查找，找到了注入成功。找不到 就报错。

**示例代码：**

``` java
@Autowired
private IUserService userServiceImpl
```



#### @Primary和@Qualifier

**作用**： 在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。

**它在给成员变量注入时不能独立使用，必须和 `@Autowire` 一起使用；但是给方法参数注入时，可以独立使用**

**属性**： value：指定 bean 的 id。

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



