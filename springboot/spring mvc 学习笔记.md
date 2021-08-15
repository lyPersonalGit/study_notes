### 处理器映射

#### I @RequestMapping

作用： 可以通过设置value和path来设置请求url，从而让对应的请求映射到控制器和方法上

属性： value/path 配置要映射的路径



### 获取控制器参数

#### I @RequestParam

作用： 用来指定HTTP参数和方法参数的映射关系，配置器就会按照其配置的映射关系来得到参数

属性： `value/name` 配置要映射的参数，`required` 设置要传入的参数是否可以为空，`defaultValue` 设置传入的值为空时默认值

``` java
@GetMapping("/test")
@ResponseBody
public Map<String, Object> test(@RequestParam("int_val") Integer intVal,
@RequestParam("long_val") Long longVal,
@RequestParam("str_val") String strVal) {
    Map<String, Object> map = new HashMap<>();
    map.put("intVal", intVal);
    map.put("longVal", longVal);
    map.put("strVal", strVal);
    return map;
}
```



#### II 获取JSON数据

**注解：@RequestBody**

作用： 标注将接受前段传来的json请求体，而json请求体与实体类的属性名称需要保持一致。

``` java
@PostMapping("/insert")
@ResponseBody
public User insert(@RequestBody User user) {
    userService.insertUser(user);
    return user;
}
```



#### III 获取路径参数

**注解：@PathVariable**

作用： 标注在参数上，获取写在路径上的参数

属性： value 配置要映射的参数。

``` java
@GetMapping("/{id}")
@ResponseBody
public User get(@PathVariable("id") Long id) {
    return userService.getUser(id);
}
```



#### IV 获取日期格式化参数和数字格式化参数

**注解：@DateTimeFormat**

作用： 标注在Date和TimeStamp类型参数上，对日期进行格式化

属性： pattern，iso

``` java
@PostMapping("/format/commit")
@ResponseBody
public Map<String, Object> format(@DateTimeFormat(pattern = "yyyy-MM-dd", iso = ISO.DATE)) {
	Map<String, Object> dataMap = new HashMap<>();
    dataMap.put("date", date);
    return dataMap;
}
```

**注解：@NumberFormat**

作用： 标注在数字类型参数上，对数字进行格式化

属性： pattern

``` java
@PostMapping("/format/commit")
@ResponseBody
public Map<String, Object> format(@NumberFormat(pattern = "#,###.##") Double number) {
	Map<String, Object> dataMap = new HashMap<>();
    dataMap.put("number", number);
    return dataMap;
}
```

**附加：格式化返回值中的日期 @JsonFormat**

作用：标注在作为返回值得日期属性上，对日期进行格式化

属性：pattern，timezone

``` java
public class UserDto {
    
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
    private Date createTime;
    
}
```



#### V 获取请求头参数

**注解：@RequestHeader**

作用：标注在参数上获取header中的参数

属性：name/value

``` java
@PostMapping("/header/user")
@ResponseBody
public User headerUser(@RequestHeader("id") Long id) {
    User user = userService.getUser(id);
    return user;
}
```



### 数据验证

#### I JSR-303

JSR-303验证主要是通过注解的方式进行的。先定义一个需要验证的POJO，此时需要在其属性中加入相关的注解。

``` java
public class InputForm {
    @NotNull(message = "id不能为空")
    private Long id;
    
    @NotNull
    @Future(message = "只能是未来的日期")
    @DateFormat(pattern = "yyyy-MM-dd")
    private Date date;
    
    @NotNull
    @DecimalMin(value = "0.1")
    @DecimalMax(value = "10000.00")
    private Double doubleValue = null;
    
    @NotNull
    @Min(value = 1, message = "最小值为1")
    @Max(value = 10, message = "最大值为10")
    private Integer interger;
    
    @Range(min = 1, max = 88, message = "范围为1至88")
    private Long range;
    
    @Size(min = 20, max = 30, message = "字符串长度要求20到30之间")
    private String size;
    
    @Email(message = "邮箱格式错误")
    private String email;
    
    /** getter and setter **/
}
```

JSR-303 是JAVA EE 6 中的一项子规范，叫做Bean Validation，Hibernate Validator 是 Bean Validation 的参考实现 . Hibernate Validator 提供了 JSR 303 规范中所有内置 constraint 的实现，除此之外还有一些附加的 constraint。

| Constraint                 | 详细信息                                                 |
| -------------------------- | -------------------------------------------------------- |
| @Null                      | 被注释的元素必须为null                                   |
| @NotNull                   | 被注释的元素必须不为null                                 |
| @AssertTrue                | 被注释的元素必须为true                                   |
| @AssertFalse               | 被注释的元素必须为false                                  |
| @Min(value)                | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)                | 被注释的元素必须是一个数字，其值必须小于等于指定的最小值 |
| @DecimalMin(value)         | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value)         | 被注释的元素必须是一个数字，其值必须小于等于指定的最小值 |
| @Size(min, max)            | 被注释的元素的大小必须在指定的范围内                     |
| @Digits(integer, fraction) | 被注释的元素必须是一个数字，其值必须在可接受的范围内     |
| @Past                      | 被注释的元素必须是一个过去的日期                         |
| @Future                    | 被注释的元素必须是一个未来的日期                         |
| @Pattern(value)            | 被注释的元素必须符合指定的正则表达式                     |

附加的 constraint

| Constraint | 详细信息                               |
| ---------- | -------------------------------------- |
| @Email     | 被注释的元素必须是电子邮箱地址         |
| @Length    | 被注释的字符串的大小必须在指定的范围内 |
| @NotEmpty  | 被注释的元素必须非空                   |
| @Range     | 被注释的元素必须在合适的范围内         |



### 文件上传

文件上传控制器可以通过HttpServletRequest、MultipartFile和Part三种传参的方法来完成文件上传。

``` java
@PostMapping("/upload/request")
@ResponseBody
public Map<String, Object> uploadRequest(HttpServletRequest request) {
    boolean flag = false;
    MultipartHttpServletRequest mreq = null;
    //强制转换为MultipartHttpServletRequest接口对象
    if (request instanceof MultipartHttpServletRequest) {
        mreq = (MultipartHttpServletRequest) request;
    } else {
        return dealResultMap(false, "上传失败");
    }
    //获取MultipartFile文件信息
    MultipartFile mf = mreq.getFile("file");
    //获取源文件名称
    String fileName = mf.getOriginalFilename();
    File file = new File(fileName);
    try {
        //保存文件
        mf.transferTo(file);
    } catch (Exception e) {
        e.printStackTrace();
        return dealResultMap(false, "上传失败");
    }
    return dealResultMap(true, "上传成功");
}

private Map<String, Object> dealResultMap(boolean success, String msg) {
    Map<String, Object> result = new HashMap<>();
    result.put("success", success);
    result.put("msg", msg);
    return result;
}
```

调用控制器之前，DispatcherServlet会将其转换为MultipartHttpServletRequest对象，所以方法中使用了强制转换，从而得到了MultipartHttpServletRequest对象，然后获取MultipartFile对象。

``` java
@PostMapping("/upload/multipart")
@ResponseBody
public Map<String, Object> uploadMultipartFile(MultipartFile file) {
    String fileName = file.getOriginalFilename();
    File dest = new File(fileName);
    try {
        file.transferTo(dest);
    } catch (Exception e) {
        e.printStackTrace();
        return dealResultMap(false, "上传失败");
    }
    return dealResultMap(true, "上传成功");
}

private Map<String, Object> dealResultMap(boolean success, String msg) {
    Map<String, Object> result = new HashMap<>();
    result.put("success", success);
    result.put("msg", msg);
    return result;
}
```

MultipartFile对象的getOriginalFilename方法可以得到上传的文件名，而通过它的transferTo方法，就可以将文件存到对应的路径中。

``` java
@PostMapping("/upload/part")
@ResponseBody
public Map<String, Object> uploadPart(Part file) {
    //获取提交文件名称
    String fileName = file.getSubmittedFileName();
    try {
        //写入文件
        file.write(fileName);
    } catch (Exception e) {
        e.printStackTrace();
        return dealResultMap(false, "上传失败");
    }
    return dealResultMap(true, "上传成功");
}

private Map<String, Object> dealResultMap(boolean success, String msg) {
    Map<String, Object> result = new HashMap<>();
    result.put("success", success);
    result.put("msg", msg);
    return result;
}
```

uploadPart方法使用的是Servlet的API，可以使用其write方法直接写入文件（推荐）。



### 拦截器

#### 拦截器方法的执行流程



#### 开发拦截器

首先所有的拦截器都需要实现HandlerInterceptor接口。

``` java
public class PageInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("处理器前方法");
        //返回true时，不会拦截后续的处理
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("处理器后方法");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("处理器完成方法");
    }
}
```



#### 注册拦截器

有了PageInterceptor这个拦截器，Spring MVC并不会发现它，它还需要进行注册才能够拦截注册器，为此需要在配置文件中实现WebMvcConfigurer接口，最后覆盖其addInterceptors方法进行注册拦截器。

``` java
@SpringBootConfiguration
public class SpringMvcConfiguration implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注册拦截器到Spring MVC机制，然后它会返回一个拦截器注册
        InterceptorRegistration registration = registry.addInterceptor(new PageInterceptor());
        //指定拦截匹配机制，限制拦截器拦截请求
        registration.addPathPatterns("/**"); //所有路径都拦截
        registration.excludePathPatterns(
            "/login",
            "/**/*.html",
            "/**/*.js",
            "/**/*.css"
        );
    }
}
```



### 增强控制器

#### I @ControllerAdvice

定义一个控制器的控制类，允许定义一些关于增强控制器的各类通知和限定增强哪些控制器功能。

``` java
@ControllerAdvice(
        //指定拦截的包
        basePackages = { "com.demo.test.controller.*" },
        //限定被标注为@Controller,@RestController的类才被拦截
        annotations = { Controller.class, RestController.class }
)
public class MyControllerAdvice {
    
}
```



#### II @InitBinder

定义控制器参数绑定规则，如转换规则，格式化等，它会在参数转换之前执行。

``` java
@InitBinder
public void initDataBinder(WebDataBinder binder) {
    //自定义日期编辑器，限定格式为yyyy-MM-dd，且参数不允许为空
    CustomDateEditor dateEditor = new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), false);
    //注册自定义日期编辑器
    binder.registerCustomEditor(Date.class, dateEditor);
}
```



#### III @ModelAttribute

可以在控制器方法执行之前，对数据模型进行操作。

``` java
@ModelAttribute
public void projectModel(Model model) {
    model.addAttribute("project_name", "demoProject");
}
```



#### IV @ExceptionHandler

定义控制器发生异常后的操作。一般来说，发生异常后，

``` java
@ExceptionHandler(value = Exception.class)
@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
@ResponseBody
public Map<String, Object> dealCustomException(HttpServletRequest request, Exception e) {
    Map<String, Object> map = new HashMap<>();
    //获取异常信息
    map.put("msg", e.getMessage());
    map.put("code", 1);
    return map;
}
```

