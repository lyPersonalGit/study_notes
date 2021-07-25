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

#### II @RequestBody

作用： 标注将接受前段传来的json请求体，而json请求体与实体类的属性名称需要保持一致。

``` java
@PostMapping("/insert")
@ResponseBody
public User insert(@RequestBody User user) {
    userService.insertUser(user);
    return user;
}
```

#### III @PathVariable

作用： 标注在参数上，获取写在路径上的参数

属性： value 配置要映射的参数。

``` java
@GetMapping("/{id}")
@ResponseBody
public User get(@PathVariable("id") Long id) {
    return userService.getUser(id);
}
```

#### IIII 获取日期格式化参数和数字格式化参数 @DateTimeFormat和@NumberFormat

**@DateTimeFormat**

作用： 标注在Date和TimeStamp类型参数上，对日期进行格式化

属性： iso

**@NumberFormat**

作用： 标注在数字类型参数上，对数字进行格式化

属性： pattern

``` java
@PostMapping("/format/commit")
@ResponseBody
public Map<String, Object> format() {

}
```

#### IIII 数据验证

                                   