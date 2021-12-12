## Using MapStruct

#### Maven中引入mapstruct:

```
...
<properties>
    <mapstruct.version>1.4.1.Final</mapstruct.version>
</properties>
...
<dependencies>
    <!--mapStruct依赖-->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${mapstruct.version}</version>
    </dependency>
    <!-- 注解处理器，根据注解自动生成mapper的实现 -->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct-processor</artifactId>
        <version>${mapstruct.version}</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                    <!-- 使用 Lombok 需要添加 -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>${lombok.version}</version>
                    </path>
                    <!-- Lombok 1.18.16 及以上需要添加，不然报错 -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok-mapstruct-binding</artifactId>
                        <version>0.2.0</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
...
```

#### 根据注解自动生成转换文件：

**需要生成的mapstruct**

``` java
public interface BaseConverter<E, D> {

    /**
     * Entity转DTO
     *
     * @param entity
     * @return
     */
    D toDto(E entity);

    /**
     * DTO转Entity
     *
     * @param dto
     * @return
     */
    E toEntity(D dto);

    /**
     * Entity集合转DTO集合
     *
     * @param entityList
     * @return
     */
    List<D> toDto(List<E> entityList);

    /**
     * DTO集合转Entity集合
     *
     * @param dtoList
     * @return
     */
    List<E> toEntity(List<D> dtoList);
}
```

**在需要生成的Entity与DTO添加自动生成注解**

``` java
@Mapper(componentModel = "spring", unmappedTargetPolicy = ReportingPolicy.IGNORE)
public interface UserConverter extends BaseConverter<User, UserDto> {
    
}
```

