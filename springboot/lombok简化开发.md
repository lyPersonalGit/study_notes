# lombok简化开发

#### 引入pom

``` xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.24</version>
    <scope>provided</scope>
</dependency>
```

#### lombok常用类注解

| 注解                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| @Getter                  | 作用类上，生成所有成员变量的getter方法；作用于成员变量上，生成该成员变量的getter方法 |
| @Setter                  | 作用类上，生成所有成员变量的setter方法；作用于成员变量上，生成该成员变量的setter方法 |
| @ToString                | 作用于类，覆盖默认的toString()方法，可以通过of属性限定显示某些字段，通过exclude属性排除某些字段 |
| @EqualsAndHashCode       | 作用于类，覆盖默认的equals和hashCode                         |
| @NoArgsConstructor       | 生成无参构造器                                               |
| @AllArgsConstructor      | 生成全参构造器                                               |
| @RequiredArgsConstructor | 生成包含final和@NonNull注解的成员变量的构造器                |
| @Data                    | 作用于类上，是以下注解的集合：@ToString @EqualsAndHashCode @Getter @Setter @RequiredArgsConstructor |
| @Builder                 | 作用于类上，供了一种基于建造者模式的构建对象的 API。使用 @Builder 注解为给我们的实体类自动生成 builder() 方法，并且直接根据字段名称方法进行字段赋值，最后使用 build()方法构建出一个实体对象。 |
| @Cleanup                 | 作用于变量，自动关闭资源，针对实现了java.io.Closeable接口的对象有效，如：典型的IO流对象 |
| @SneakyThrows            | 作用于方法，主要用于在没有 throws 关键字的情况下，隐蔽地抛出受检查异常，为我们平常开发中需要异常抛出时省去的 throw 操作 |
| @Accessors               | 注解在类型，用于控制生成getter和setter方法的形式             |

#### lombok日志注解

| 注解        | 等价效果                                                     |
| ----------- | ------------------------------------------------------------ |
| @CommonsLog | private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class); |
| @Flogger    | private static final com.google.common.flogger.FluentLogger log = com.google.common.flogger.FluentLogger.forEnclosingClass(); |
| @JBosLog    | private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class); |
| @Log        | private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName()); |
| @Log4j      | private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class); |
| @Log4j2     | private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class); |
| @Slf4j      | private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class); |
| @XSlf4j     | private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class); |



#### @Accessors用法

通过该注解可以控制getter和setter方法的形式。

- fluent 若为true，则getter和setter方法的方法名都是属性名，且setter方法返回当前对象

``` java
@Data
@Accessors(fluent = true)
class User {
    private Integer id;
    private String name;
    
    // 生成的getter和setter方法如下，方法体略
    public Integer id(){}
    public User id(Integer id){}
    public String name(){}
    public User name(String name){}
}
```

- chain 若为true，则setter方法返回当前对象

``` java
@Data
@Accessors(chain = true)
class User {
    private Integer id;
    private String name;
    
    // 生成的setter方法如下，方法体略
    public User setId(Integer id){}
    public User setName(String name){}
}
```

- prefix 若为true，则getter和setter方法会忽视属性名的指定前缀（遵守驼峰命名）

``` java
@Data
@Accessors(prefix = "f")
class User {
    private Integer fId;
    private String fName;
    
    // 生成的getter和setter方法如下，方法体略
    public Integer id(){}
    public void id(Integer id){}
    public String name(){}
    public void name(String name){}
}
```



#### @Cleanup用法

@Cleanup 用于标记需要释放清理操作的资源对象变量，如 FileInputStream, FileOutputStream 等，标记之后资源对象使用完毕后，就会被自动关闭和清理，实际上这里 Lombok 实现效果与 Java7 特性 try with resource 一样, 为我们屏蔽了关闭资源的模板代码。

``` java
public static void main(String[] args) throws FileNotFoundException {
        
    @Cleanup InputStream in = new FileInputStream(args[0]);
    @Cleanup OutputStream out = new FileOutputStream(args[1]);
    byte[] b = new byte[10000];
    while (true) {
        int r = 0;
        try {
            r = in.read(b);
        } catch (IOException e) {
            e.printStackTrace();
        }
        if (r == -1) {
            break;
        }
        try {
            out.write(b, 0, r);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

