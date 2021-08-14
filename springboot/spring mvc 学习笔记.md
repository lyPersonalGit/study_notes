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



#### IIII 获取日期格式化参数和数字格式化参数

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



### 给控制器增加通知

#### I 拦截指定控制器

#### II 给控制器参数绑定规则

#### III 执行方法前对数据模型进行操作

#### IIII 控制器异常捕获

