### 一、什么是TKMybatis

TKMybatis 是基于 Mybatis 框架开发的一个工具，内部实现了对单表的基本数据操作，只需要简单继承 TKMybatis 提供的接口，就能够实现无需编写任何 sql 即能完成单表操作。



### 二、TKMybatis的引用

**在pom文件中添加引用**

``` xml
<!-- tk.mybatis引用 -->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.1.5</version>
</dependency>
```

**在启动类中配置@MapperScan扫描**

``` java
@SpringBootApplication
@MapperScan("com.cqcca.data.transfer.modules.*.mapper")
public class DataTransferApplication {

    public static void main(String[] args) {
        SpringApplication.run(DataTransferApplication.class, args);
    }

}
```



### 三、TKMybatis在springboot项目中的使用

#### 3.1 配置实体类

**实体类代码示例**

``` java
@Entity
@Table(name = "user")
public class User extends BaseEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", nullable = false)
    private Long id;

    @Column(name = "username")
    private String username;

    @Column(name = "password")
    private String password;

    @Column(name = "gender")
    private String gender;

    @Column(name = "age")
    private String age;

    @Column(name = "address")
    private String address;

}
```

**在实体类中，常用的注解和意义为：**

**@Table：**描述数据库表信息，主要属性有name(表名)、schema、catalog、uniqueConstraints等。

**@Id：**指定表主键字段，无属性值。

**@Column：**描述数据库字段信息，主要属性有name(字段名)、columnDefinition、insertable、length、nullable(是否可为空)、precision、scale、table、unique、updatable等。

**@ColumnType：**描述数据库字段类型，可对一些特殊类型作配置，进行特殊处理，主要属性有jdbcType、column、typeHandler等。

其他注解如：@Transient、@ColumnResult、@JoinColumn、@OrderBy、@Embeddable等暂不描述



#### 3.2 在dao类中使用

**dao类代码示例**

``` java
public interface UserMapper extends Mapper<User> {
}
```

在Mapper接口内部已经封装了基本的单表操作，源码如下：

``` java
package tk.mybatis.mapper.common;

import tk.mybatis.mapper.annotation.RegisterMapper;

@RegisterMapper
public interface Mapper<T> extends BaseMapper<T>, ExampleMapper<T>, RowBoundsMapper<T>, Marker {
}
```

在BaseMapper接口内部已经封装了查增改删操作的接口，源码如下：

``` java
package tk.mybatis.mapper.common;

import tk.mybatis.mapper.annotation.RegisterMapper;
import tk.mybatis.mapper.common.base.BaseDeleteMapper;
import tk.mybatis.mapper.common.base.BaseInsertMapper;
import tk.mybatis.mapper.common.base.BaseSelectMapper;
import tk.mybatis.mapper.common.base.BaseUpdateMapper;

@RegisterMapper
public interface BaseMapper<T> extends BaseSelectMapper<T>, BaseInsertMapper<T>, BaseUpdateMapper<T>, BaseDeleteMapper<T> {
}
```



#### 3.3 在service层中的使用

**service层代码示例**

``` java
@Service
@RequiredArgsConstructor
public class UserServiceImpl implements UserService {

    private final UserMapper userMapper;

    @Override
    public PageResult query(UserQueryCriteria queryCriteria, int page, int size) {
        PageHelper.startPage(page, size);
        Example example = new Example(User.class);
        Example.Criteria criteria = example.createCriteria();
        List<User> list = userMapper.selectByExample(example);
        PageInfo<User> pageInfo = new PageInfo<>();
        // copy为dtoList
        List<UserDTO> dtoList = new ArrayList<>();
        UserDTO target = null;
        for (User source : list) {
            target = new UserDTO();
            BeanUtils.copyProperties(source, target);
            dtoList.add(target);
        }
        return PageResult.formPage(pageInfo);
    }

    @Override
    public List<UserDTO> query(UserQueryCriteria queryCriteria) {
        Example example = new Example(User.class);
        Example.Criteria criteria = example.createCriteria();
        List<User> list = userMapper.selectByExample(example);
        // copy为dtoList
        List<UserDTO> dtoList = new ArrayList<>();
        UserDTO target = null;
        for (User source : list) {
            target = new UserDTO();
            BeanUtils.copyProperties(source, target);
            dtoList.add(target);
        }
        return dtoList;
    }

    @Override
    public void create(User resources) {
        userMapper.insertSelective(resources);
    }
}
```

**service层中的常用操作方法**

| **操作** | **类型**                                         | **介绍**                                                     |
| -------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 增加     | Mapper.insert(record)；                          | 保存一个实体，null的属性也会保存，不会使用数据库默认值       |
|          | Mapper.insertSelective(record)；                 | 保存一个实体，忽略空值，即没提交的值会使用使用数据库默认值   |
|          |                                                  |                                                              |
| 删除     | Mapper.delete(record);                           | 根据实体属性作为条件进行删除，查询条件使用等号               |
|          | Mapper.deleteByExample(example)                  | 根据Example条件删除数据                                      |
|          | Mapper.deleteByPrimaryKey(key)                   | 根据主键字段进行删除，方法参数必须包含完整的主键属性         |
|          |                                                  |                                                              |
| 修改     | Mapper.updateByExample(record,example)           | 根据Example条件更新实体`record`包含的全部属性，null值会被更新 |
|          | Mapper.updateByExampleSelective(record, example) | 根据Example条件更新实体`record`包含的不是null的属性值        |
|          | Mapper.updateByPrimaryKey(record)                | 根据主键更新实体全部字段，null值会被更新                     |
|          | Mapper.updateByPrimaryKeySelective(record)       | 根据主键更新属性不为null的值                                 |
|          |                                                  |                                                              |
| 查询     | Mapper.select(record)                            | 根据实体中的属性值进行查询，查询条件使用等号                 |
|          | Mapper.selectAll()                               | 查询全部结果                                                 |
|          | Mapper.selectByExample(example)                  | 根据Example条件进行查询                                      |
|          | Mapper.selectByPrimaryKey(key)                   | 根据主键字段进行查询，方法参数必须包含完整的主键属性，查询条件使用等号 |
|          | Mapper.selectCount(record)                       | 根据实体中的属性查询总数，查询条件使用等号                   |
|          | Mapper.selectCountByExample(example)             | 根据Example条件进行查询总数                                  |
|          | Mapper.selectOne(record)                         | 根据实体中的属性进行查询，只能有一个返回值，有多个结果是抛出异常，查询条件使用等号。但是如果存在某个属性为int，则会初始化为0。可能影响到实际使用 |
