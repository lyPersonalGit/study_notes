# dependencyManagement



* Maven使用dependencyManagement元素来提供了一种管理依赖版本号的方式。

* 通常会在一个组织或者项目的最顶层的父POM中看到dependencyManagement元素。



使用pom.xml 中的dependencyManagement元素能让所有在子项目中引用一个依赖而不用显式的列出版本号，Maven会沿着父子层次向上走，直到找到一个拥有dependencyManagement元素的项目，然后它就会使用这个dependencyManagement元素中指定的版本号。



例如在父项目里：

```xml
<dependencyManagement>
        <dependencies>
            <!-- mysql依赖 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.26</version>
                <scope>runtime</scope>
            </dependency>
		</dependencies>
</dependencyManagement>
```

然后在子项目里就可以添加mysql-connector时就可以不指定版本号，例如：

``` xml
<dependencies>
    <!-- mysql依赖 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```



这样做的好处就是：如果有多个项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或者切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改；另外如果某个子项目需要另一个版本，只需要声明version即可。