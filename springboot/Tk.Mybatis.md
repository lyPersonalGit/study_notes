一、什么是 TKMybatis
TKMybatis 是基于 Mybatis 框架开发的一个工具，内部实现了对单表的基本数据操作，只需要简单继承 TKMybatis 提供的接口，就能够实现无需编写任何 sql 即能完成单表操作。

 

二、TKMybatis 使用
2.1 Springboot 项目中加入依赖

``` xml
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper-spring-boot-starter</artifactId>
    <version>2.0.4</version>
</dependency>
```

POJOl



2.2 使用讲解
2.2.1 实体类中使用
在实体类中，常用的注解和意义为：

@Table：描述数据库表信息，主要属性有name(表名)、schema、catalog、uniqueConstraints等。

@Id：指定表主键字段，无属性值。

@Column：描述数据库字段信息，主要属性有name(字段名)、columnDefinition、insertable、length、nullable(是否可为空)、precision、scale、table、unique、updatable等。

@ColumnType：描述数据库字段类型，可对一些特殊类型作配置，进行特殊处理，主要属性有jdbcType、column、typeHandler等。

其他注解如：@Transient、@ColumnResult、@JoinColumn、@OrderBy、@Embeddable等暂不描述