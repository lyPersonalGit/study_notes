# swagger2的配置以及注解

## swagger配置以及注解

依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

## 一、配置

常用配置参数表：

| 类型    | 名称                         | 方法                           | 作用                 |
| ------- | ---------------------------- | ------------------------------ | -------------------- |
| ApiInfo | ApiInfo                      | apiInfo()                      | 配置api的基本信息    |
| String  | groupName                    | groupName()                    | 配置组的名称         |
| boolean | enabled                      | enabled()                      | swagger的开关        |
| boolean | applyDefaultResponseMessages | applyDefaultResponseMessages() | 是否使用默认响应消息 |
| String  | host                         | host()                         | 请求路径信息         |

### 1.apiInfo配置

对apiInfo的配置需要借助于ApiInfoBuilder类来重构，也就是利用它的build()方法来实例化apiInfo

我们去看一下ApiInfoBuilder的源码，有那些可配置的参数：

```java
private String title;
private String description;
private String termsOfServiceUrl;
private Contact contact;
private String license;
private String licenseUrl;
private String version;
```

然后去看下ApiInfo类的默认配置，基本上就知道了配置的位置在哪里了

```java
public static final ApiInfo DEFAULT = new ApiInfo("Api Documentation","Api Documentation", "1.0", "urn:tos",DEFAULT_CONTACT, "Apache 2.0", "http://www.apache.org/licenses/LICENSE-2.0", new ArrayList<VendorExtension>());
```

#### 1.1 apiInfo参数表

| 类型    | apiInfo参数名     | 作用         | 默认值                                     |
| ------- | ----------------- | ------------ | ------------------------------------------ |
| String  | title             | 标题         | Api Documentation                          |
| String  | description       | 描述         | Api Documentation                          |
| String  | termsOfServiceUrl | 服务条款网址 | urn:tos                                    |
| String  | license           | 许可         | Apache 2.0                                 |
| String  | licenseUrl        | 许可链接     | http://www.apache.org/licenses/LICENSE-2.0 |
| String  | version           | 版本         | 1.0                                        |
| Contact | Contact           | 维护人信息   | null                                       |

apiInfo配置中，有一个Contact的属性

#### 1.2 contact参数表

| 类型   | 参数名 | 作用          | 默认值 |
| ------ | ------ | ------------- | ------ |
| String | name   | 维护人姓名    | “”     |
| String | url    | 维护人url地址 | “”     |
| String | email  | 维护人邮箱    | “”     |

#### 1.3 配置例子

```java
 private ApiInfo apiInfo() {
     return new ApiInfoBuilder()
         //标题
         .title("lzl的swagger案例Demo")
         //描述
         .description("用于讲解关于swagger的基本配置使用以及注解")
         //许可
         .license("lzl 2021/06/13")
         //许可链接
         .licenseUrl("https://blog.csdn.net/qq_26103133?spm=1001.2101.3001.5343")
         //服务条款网址
         .termsOfServiceUrl("www.spring.io")
         //版本
         .version("1.0")
         //维护人信息
         .contact(new Contact("lzl","www.lzl.com","2243716932@qq.com"))
         .build();
 }
```

### 2. groupName配置

这个注解用于配置当前docket的名称，每一个docket都相当于一个组

比如说这个docket只控制登陆相关的controller，配置这个属性就可以找到相应的注解类

由于这个是String类型，所以我们直接进行使用就可以了

```java
//docket1是显示auto的
@Bean
public Docket docket1(){
    return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .groupName("二组").select()
        .apis(RequestHandlerSelectors.basePackage("com.example.jyoa.controller"))
        .paths(PathSelectors.ant("/auto/**")).build();
}
//docket2是显示admin的
@Bean
public Docket docket2(){
    return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .groupName("二组").select()
        .apis(RequestHandlerSelectors.basePackage("com.example.jyoa.controller"))
        .paths(PathSelectors.ant("/admin/**")).build();
}
```



### 3. enabled配置

这个是Swagger开关，源码描述是默认为true，如果为false就不能能在浏览器中访问了

```java
private boolean enabled = true;
```



### 4. applyDefaultResponseMessages配置

这个是应用默认响应消息,默认值为true

如果为false的话就会启用ResponseMessages

然后通过调用globalResponseMessage()写入自己的响应消息

源码如下：

```java
//默认为true
private boolean applyDefaultResponseMessages = true;

//当 applyDefaultResponseMessages = false 时,这个就会启用
private final Map<RequestMethod, List<ResponseMessage>> responseMessages = newHashMap();


//然后就要你自己配置调用globalResponseMessage()，他的写入源码
public Docket globalResponseMessage(RequestMethod requestMethod,
                                    List<ResponseMessage> responseMessages) {

    this.responseMessages.put(requestMethod, responseMessages);
    return this;
}
```



### 5. host

这个是文档的host信息，在swagger中测试的时候，访问的路径就是通过host属性来控制的

```java
private String host = "";
//默认值为空，
//但是实际上显示的值为项目地址也就是：localhost:8080/
```



### 6.完整配置

```java
@Configuration
@EnableSwagger2
@Profile({"test"})
public class SwaggerConfig {

    @Value("${swagger.enable}")
    private boolean enable;

    @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("二组").select()
                .apis(RequestHandlerSelectors.basePackage("com.example.jyoa.controller"))
                .build();
    }
    
    @Bean
    public Docket docket2(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("一组").select().build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                //标题
                .title("lzl的swagger案例Demo")
                //描述
                .description("用于讲解关于swagger的基本配置使用以及注解")
                //许可
                .license("lzl 2021/06/13")
                //许可链接
                .licenseUrl("https://blog.csdn.net/qq_26103133?spm=1001.2101.3001.5343")
                //服务条款网址
                .termsOfServiceUrl("www.spring.io")
                //版本
                .version("1.0")
                //维护人信息
                .contact(new Contact("lzl","www.lzl.com","2243716932@qq.com"))
                .build();
    }
}
```

## 二、注解

swagger常用注解表

| 名称               | 使用           | 描述                       |
| ------------------ | -------------- | -------------------------- |
| @Api               | controller类上 | 对请求类的说明             |
| @ApiIgnore         | controller类上 | 隐藏类swagger不会显示      |
| @ApiModel          | 实体类上       | 说明实体类的用途           |
| @ApiModelProperty  | 实体类参数上   | 说明实体类属性的的含议     |
| @JsonIgnore        | 实体类参数上   | 隐藏该实体类属性           |
| @ApiOperation      | 方法上         | 方法的说明                 |
| @ApiImplicitParams | 方法上组合使用 | 方法的参数的说明           |
| @ApiImplicitParam  | 方法上         | 用于指定单个参数的说       |
| @ApiResponses      | 方法上组合使用 | 方法返回值状态码的说明     |
| @ApiResponse       | 方法上         | 指定单个返回值状态码的说明 |
| @ApiParam          | 方法参数上     | 单个方法参数               |

显示详细信息



### 1. @Api 说明controller类

@api的常用参数表

| 类型            | 名称           | 描述     |
| --------------- | -------------- | -------- |
| String[]        | tags           | 标题分组 |
| String          | description    | 描述     |
| String          | produces       | 输出     |
| String          | consumes       | 输入     |
| String          | protocols      | 请求规范 |
| Authorization[] | authorizations | 安全设置 |
| boolean         | hidden         | 是否启用 |

没写上的都停用了，没讲的必要

#### 1.1 tags 标题

就是当前controller类的标题分组，是数组类型的可以写多个标题。例如：登陆模块、注册模块

#### 1.2 description 描述

一般用于对标题的补充说明。例如：包含注册接口、登陆接口、权限控制接口等等

#### 1.3 produces 输出规范

源码描述是这样子的：

```java
/**
     * Corresponds to the `produces` field of the operations under this resource.
     * <p>
     * Takes in comma-separated values of content types.
     * For example, "application/json, application/xml" would suggest the operations
     * generate JSON and XML output.
     * <p>
     * For JAX-RS resources, this would automatically take the value of the {@code @Produces}
     * annotation if such exists. It can also be used to override the {@code @Produces} values
     * for the Swagger documentation.
     *
     * @return the supported media types supported by the server, or an empty string if not set.
     */
String produces() default "";
```

大致意思就是说，你可以通过他控制输出的类型，"application/json, application/xml"是输出json和·xml类型

但是我没用过

#### 1.4 consumes 输入规范

和produces 是一样的，只不过这个是输入的规范，还是没用过

#### 1.5 authorizations 安全控制

这个我也没用过，因为我个人很少通过security去搞安全，一般都是直接@Profile，然后通过控制版本来实现的，这个比较简单方便，正式上线的时候通过shiro直接把链接封掉就好了

```java
	/**
     * Corresponds to the `security` field of the Operation Object.
     * <p>
     * Takes in a list of the authorizations (security requirements) for the operations under this resource.
     * This may be overridden by specific operations.
     *
     * @return an array of authorizations required by the server, or a single, empty authorization value if not set.
     * @see Authorization
     */
Authorization[] authorizations() default @Authorization(value = "");
```

#### 1.6 hidden

控制当前类是否在swagger中显示，还是没用过，而且我在写这个的时候实验了一下，好像没作用

```java
/**
     * Hides the operations under this resource.
     *
     * @return true if the api should be hidden from the swagger documentation
     */
    boolean hidden() default false;
```

#### 1.7 完整配置

综上所述我们api注解中常用的配置就是两个：tags，description，其他的基本上也都不怎么用

```java
@Api(tags = {"登陆模块"},description = "相当牛逼的东西")
```



### 2. @ApiIgnore 隐藏controller类

 标注了该注解之后，扫描controller类就会忽略掉这个类，在swagger-ui里面就看不到了

 这个注解只有一个参数，这个参数value()外面是看不到的，只能在代码里面看到，相当于注释

```java
 /**
   * A brief description of why this parameter/operation is ignored
   * @return  the description of why it is ignored
   */
  String value() default "";
```

 使用这个注解时我们不需要填写参数

```java
@ApiIgnore
//直接将注解放在类上就可以使用了
```



### 3. @ApiModel 说明实体类

 用于说明实体类的用途

 这个注解的参数还是蛮多的，但是常用的就两个：value和description

 value是显示在Models中，该实体类叫什么名字，description还是描述

```java
@ApiModel(value = "admin-auto" ,description = "管理员表")
```



### 4. @ApiModelProperty 说明实体类属性

 用与说明实体类的属性，这个注解的参数还是蛮多的，这些注解效果需要在swagger-ui的models里面查看

 常用参数表：

| 类型    | 名称            | 描述     |
| ------- | --------------- | -------- |
| String  | value           | 描述     |
| String  | allowableValues | 许可值   |
| String  | dataType        | 数据类型 |
| boolean | required        | 是否需要 |
| int     | position        | 排序     |
| boolean | hidden          | 隐藏属性 |
| String  | example         | 示例值   |
| boolean | readOnly        | 标记只读 |
| boolean | allowEmptyValue | 允许为空 |

#### 4.1 value描述

一般写这个参数是干嘛的，

例如：

```java
    @ApiModelProperty(value = "用户账号")
    private String username;
```

#### 4.2 allowableValues 许可值

一般用于标注数值范围，这个只是提示，但是你不按提示的输入一样能输入的，只是给前段测试一个提示

例如：性别，后台是0代表女1代表男，你就可以写提示上去，限制0,1两个数

```java
//标记输入值，在swagger页面显示上就会出现这个参考值为0和1
@ApiModelProperty(allowableValues="0,1" )
private int age;

//标记范围，1-99之间的数值
@ApiModelProperty(allowableValues="range(1,99)" )
private int age;

//标记范围1-无穷大
@ApiModelProperty(allowableValues="range(1,infinity)" )
private int age;
```

#### 4.3 dataType 数据类型

用于覆盖默认的数据类型，没用过

源码这么写的：

```java
 /**
     * The data type of the parameter.
     * <p>
     * This can be the class name or a primitive. The value will override the data type as read from the class
     * property.
     */
    String dataType() default "";
```

#### 4.4 required 是否需要

告诉前端该参数为必填项

```java
//为true后swagger-ui就会显示该参数为必填选项
@ApiModelProperty(required = true)
private int age;
```

#### 4.5 position 排序

对属性显示顺序进行排序

下面代码显示顺序就是

1.username

2.age

3.sex

```java
@ApiModelProperty(value = "登陆账号",position = 0)
private String username;

@ApiModelProperty(required = true,position = 2)
private String sex;

@ApiModelProperty(required = true,position = 1)
private int age;
```

#### 4.6 hidden 是否隐藏

该值如果为true就隐藏该属性 和 @JsonIgnore类似

```java
@ApiModelProperty(required = truehidden = true)
private int age;
```

#### 4.7 example

赋予属性一个默认值，当然类型是String类型的，你可以理解为文本

```java
@ApiModelProperty(example ="123")
private int age;
```

#### 4.8 readOnly

标记只读，字面意思

```java
@ApiModelProperty(readOnly=true)
private int age;
```

表现出来的效果就是这样子的，展现出来的默认值

```html
post请求
{
  "username": "string",
  "sex": "string"
}

get请求
{
  "age": 0,
  "username": "string",
  "sex": "string"
}
```

当然你要是强行要写，他也是能写的，只是他不会给你自动展示那个字段，要你自己去手写

#### 4.9 allowEmptyValue

标记该字段是否可以为空，同理也只是提示，你要是后台没写控制代码，一样能传空值

```java
@ApiModelProperty(allowEmptyValue=true)
private int age;
```



### 5. @ JsonIgnore 隐藏实体类属性

和 hidden=true 类似，用于隐藏该实体类字段，标注在实体类的参数上

```java
@JsonIgnore
@ApiModelProperty(required = true,position = 2)
private String sex;
```



### 6. @ApiOperation 方法的说明

@ApiOperation 常用参数表：

| 类型     | 名称              | 描述             |
| -------- | ----------------- | ---------------- |
| String   | value             | 对方法的简要说明 |
| String   | notes             | 对方法的详细说明 |
| String[] | tags              | 分组标题         |
| String   | responseContainer | 包装响应容器     |
| String   | httpMethod        | method字段值     |
| boolean  | hidden            | 是否隐藏         |
| int      | code              | code状态码       |
| String   | produces          | 输出格式         |
| String   | consumes          | 输入格式         |

#### 6.1 value 简要说明

这个参数是对方法的简要说明，源码写的是：

> Provides a brief description of this operation. Should be 120 characters or less

建议在120字以下，不过应该也没人拿简要说明写作文吧？

例如：登陆方法

#### 6.2 notes 详细说明

这个参数是对简要说明的的补充说明，详细说明

例如：包含了微信登陆，QQ登陆，短信登陆，账号密码登陆的登陆方法，返回一个Admin实体类

```java
@ApiOperation(value = "登陆方法",notes = "微信登陆，QQ登陆，短信登陆，账号密码登陆的登陆方法，返回一个Admin实体类")
```

#### 6.3 tags 分组标题

每一个tags都是一个标题分组，你可以如果在加上了这个参数，参数值是没有的标题分组就会新建一个

已有的标题分组就会加入进去，例如：有两个controller类

```java
//一个标题分组是ao模块 
@RestController
@RequestMapping("ao")
@Api(tags = {"ao模块"},description = "非常牛逼的东西",basePath = "www.baidu.com",hidden = true,produces = "application/xml")
public class AOController {

    @Autowired
    AutoService autoService;
    
    @ApiOperation(value = "登陆方法",notes = "微信登陆，QQ登陆，短信登陆，账号密码登陆的登陆方法，返回一个Admin实体类",tags ="登陆模块" )
    @RequestMapping(value = "/set",method = RequestMethod.GET)
    public AdminEntity set( String name){
        AdminEntity adminEntity=autoService.getAll();
        return adminEntity;
    }
}
----------------------------------------------------------------
//一个是登陆模块
@RestController
@RequestMapping("auto")
@Api(tags = {"登陆模块"},description = "相当牛逼的东西")
public class AutoController {

    @RequestMapping(value = "/set",method = RequestMethod.POST)
    public AutoEntity setAll(@RequestBody AutoEntity autoEntity){
        List<Integer> list = new ArrayList<>();
        list.add(123);
        list.get(0);
        return autoEntity;
    }
}
```

那么他们呈现出来的结果就是，ao模块有一个方法/ao/set ，登陆模块有两个方法/ao/set方法以及/auto/set方法

#### 6.4 responseContainer 包装响应容器

声明响应容器，没用过，给兄弟们看源码

```java
//Declares a container wrapping the response.
//Valid values are "List", "Set" or "Map". Any other value will be ignored.
String responseContainer() default "";
```

#### 6.5 httpMethod method字段值

这个参数如果你不写，他就会默认读取方法上的注解，就会出现一种情况，你直接使用@RequestMapping注解的

时候没有声明method类型，就会直接出来"GET", “HEAD”, “POST”, “PUT”, “DELETE”, “OPTIONS” ,“PATCH”

很多种类型的接口文档。而且即使是你声明了method类型，然后在用这个参数的话，参数会覆盖掉你声明的类型

例如：

```java
@ApiOperation(value = "德莱文",responseContainer="Map",httpMethod="PUT")
@RequestMapping(value = "/set",method = RequestMethod.POST)
public AutoEntity setAll(@RequestBody AutoEntity autoEntity){
    List<Integer> list = new ArrayList<>();
    list.add(123);
    list.get(0);
    return autoEntity;
}
```

呈现出来的结果为PUT类型

#### 6.6 hidden 是否隐藏

该参数用于隐藏该controller类的该方法，标注之后就不在swagger-ui里面显示了

```java
@ApiOperation(value = "德莱文",hidden = true)
@RequestMapping(value = "/set",method = RequestMethod.POST)
public AutoEntity setAll(@RequestBody AutoEntity autoEntity){
    List<Integer> list = new ArrayList<>();
    list.add(123);
    list.get(0);
    return autoEntity;
}
```

这样做的效果就是，swagger-ui不会显示这个方法

#### 6.7 code code状态码

这个不需要改，看看就行了,默认值为200

```java
 /**
 The HTTP status code of the response.
 The value should be one of the formal <a target="_blank" href="http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html">HTTP Status Code Definitions</a>.
     */
    int code() default 200;
```

#### 6.8 produces 输出格式

标注输出格式，表示以什么类型输出，但是只是表示，提示前端而已，例如：

```
application/json, application/xml
```

#### 6.9 consumes 输入格式

标注输入格式，与produces 一样



### 7. @ApiImplicitParam方法参数说明

@ApiImplicitParam常用参数表：

| 属性     | 名称            | 描述         |
| -------- | --------------- | ------------ |
| String   | name            | 名称         |
| String   | value           | 简要描述     |
| String   | defaultValue    | 描述默认值   |
| String   | allowableValues | 许可值       |
| boolean  | required        | 是否需要     |
| boolean  | allowMultiple   | 是否为集合   |
| String   | dataType        | 参数数据类型 |
| Class<?> | dataTypeClass   | 参数类       |
| String   | paramType       | 参数类型     |
| String   | example         | 非实体类例子 |
| Example  | examples        | 实体类例子   |
| boolean  | allowEmptyValue | 允许为空     |
| boolean  | readOnly        | 只读         |

显示详细信息

#### 7.1 name 名称

这个属性就是名字，推荐是和形参写一样的，源码描述是这样的

```java
/*
Name of the parameter. 
For proper Swagger functionality, follow these rules when naming your parameters based on {@link #paramType()}: 
If {@code paramType} is "path", the name should be the associated section in the path. 
For all other cases, the name should be the parameter name as your application expects to accept. 
*/
```

#### 7.2 value 简要描述

字面意思

#### 7.3 defaultValue 默认值

字面意思

#### 7.4 allowableValues 许可值

这个和@ApiModelProperty 的allowableValues 属性是一样的，写法也是一样的

#### 7.5 required 是否需要

默认为false，为true就提示为必填参数，当然只是提示

#### 7.6 allowMultiple 是否为集合

在我的理解中是集合的意思，源码描述是这样的。是否允许参数出现多次接收多个值

默认为false，为true就是允许

> pecifies whether the parameter can accept multiple values by having multiple occurrences.

#### 7.7 dataType 参数数据类型

这个基本上是不写的，但是也有特殊情况，比如说我们这么写，传参的时候name，传过来就是int类型

你不写的话，就会默认读取你的参数类型，写了就会覆盖掉

```java
@ApiOperation(value = "登陆方法",notes = "微信登陆，QQ登陆，短信登陆，账号密码登陆的登陆方法，返回一个Admin实体类",tags ="登陆模块" )
@ApiImplicitParam(name = "name",defaultValue = "wdnmd",dataType = "int")
@RequestMapping(value = "/set",method = RequestMethod.POST)
public AdminEntity set( String name){
    
    AdminEntity adminEntity=autoService.getAll();
    return adminEntity;
}
```

然后控制台就会报错，就会：

> Illegal DefaultValue wdnmd for parameter type integer

#### 7.8 dataTypeClass 参数实体类

这个和dataType 类似，只不过一个是基本数据类型一个是实体类

#### 7.9 paramType 参数类型

指定参数请求类型，有效值是path，query， body， header，form，例如

```java
@ApiImplicitParam(name = "name",defaultValue = "wdnmd",paramType="body")
@RequestMapping(value = "/set",method = RequestMethod.POST)
public AdminEntity set( String name){

    AdminEntity adminEntity=autoService.getAll();
    return adminEntity;
}
```

#### 7.10 example 非实体类例子

这个是例子，也就是要postman时候的默认例子，前面的那个defaultValue 默认值是显示的默认值

这两个是不一样的，侧重点不一样，这个如果不写，那默认例子值就是defaultValue 的值

#### 7.11 examples 实体类例子

实体类例子的时候，就用这个属性，不使用defaultValue 属性，

当然我也没用过，我一般都是@RequestBody，不会直接接收实体类类型

#### 7.12 allowEmptyValue 允许为空

字面意思，是否允许空值

#### 7.13 readOnly 只读

写入的时候，swagger-ui不会给你显示这个参数，当然你可以手动写上去



### 8.@ApiImplicitParams注解

这个注解是与@ApiImplicitParam注解结合使用的，无法单独使用

例如：

```java
@ApiOperation(value = "登陆方法",notes = "微信登陆，QQ登陆，短信登陆，账号密码登陆的登陆方法，返回一个Admin实体类",tags ="登陆模块" )
@ApiImplicitParams({
        @ApiImplicitParam(name = "name",defaultValue = "wdnmd",value="账号"),
        @ApiImplicitParam(name = "pass",defaultValue = "123456",value="密码")
})
@RequestMapping(value = "/set",method = RequestMethod.POST)
public Object set(@RequestParam("name") String name,@RequestParam("pass")String pass){
    System.out.println(name);
    System.out.println(pass);

    return pass;
}
```



### 9.@ApiResponse注解 返回值状态码

该注解用于描述状态码

主要参数就两个 ：code 状态码以及message 描述信息

例如：

```java
@ApiResponse(code = 200,message = "成功无错")
```



### 10.@ApiResponses注解

这个与@ApiImplicitParams相似，需要和@ApiResponse结合使用

例如：

```java
@ApiResponses({
        @ApiResponse(code = 200,message = "成功无错"),
        @ApiResponse(code = 404,message = "没找到"),
        @ApiResponse(code = 405,message = "不知道什么错"),
})
```



### 11.@ApiParam注解 单个参数说明

这个参数的属性与@ApiImplicitParam是类似的，只是写法上不同

@ApiImplicitParam是写在方法上，他是写在属性前，但是不太推荐这种写法

```java
public Object set(@RequestParam("name")
                  @ApiParam(value = "username") String name,
                  @RequestParam("pass")String pass){
    System.out.println(name);
    System.out.println(pass);

    return pass;
}
```