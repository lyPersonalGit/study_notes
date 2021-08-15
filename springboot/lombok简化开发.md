#### 引入pom

``` xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.16.18</version>
    <scope>provided</scope>
</dependency>
```

#### lombok常用注解

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
| @Cleanup                 | 自动关闭资源，针对实现了java.io.Closeable接口的对象有效，如：典型的IO流对象 |
| @Slf4j                   | 注解在类上，根据不同的注解生成不同类型的log对象，但是实例名称都是log，有六种可选实现类 |

