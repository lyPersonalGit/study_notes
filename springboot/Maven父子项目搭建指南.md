# Maven父子项目搭建指南



### 1. New Project

创建Maven项目，确定jdk版本为1.8，勾选从原型创建，选择maven-archetype-site原型。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\New-Project.png" alt="New Project" style="zoom:80%;" />

maven提供的41个骨架原型见附录。

### 2. 项目命名

为项目命名，选择项目所在目录，并设置项目的GAV（groupId,artifactId,version）。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\Named-Project.png" alt="Named-Project" style="zoom:80%;" />

### 3. 选择maven版本

选用本地安装的maven与引用的repository。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\Select-Maven.png" alt="Select-Maven" style="zoom:80%;" />

### 4.修改字符编码

将项目的文件编码修改为UTF-8。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\Encoding.png" alt="Encoding" style="zoom:80%;" />

### 5. 注解生效

注解生效激活。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\Annotation.png" alt="Annotation" style="zoom:80%;" />

### 6. Java编译版本

java编译版本选8。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\Java-Compile-Version.png" style="zoom:80%;" />

### 7. File Type过滤

将`*.idea`、`*.iml`等不要展示的文件隐藏掉。

<img src="D:\personal\study_notes\springboot\Maven父子项目搭建指南\File-Type.png" style="zoom:80%;" />

### 8. 编写父工程POM文件

pom.xml文件示例如下：

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cqcca.data.transfer</groupId>
    <artifactId>data-transfer-system</artifactId>
    <version>1.0</version>
    <packaging>pom</packaging>
    
    <modules>
        <module>data-transfer-common</module>
        <module>data-transfer-module</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencyManagement>
        <dependencies>
            <!-- mysql依赖 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.26</version>
                <scope>runtime</scope>
            </dependency>
            <!--工具包-->
            <dependency>
                <groupId>cn.hutool</groupId>
                <artifactId>hutool-all</artifactId>
                <version>5.3.4</version>
            </dependency>
            <!-- springboot引用 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.5.4</version>
            </dependency>
            <!--Spring boot Redis-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-data-redis</artifactId>
                <version>2.5.4</version>
            </dependency>
            <!-- web模板引用 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-thymeleaf</artifactId>
                <version>2.5.4</version>
            </dependency>
            <!-- JSR-303数据校验 -->
            <dependency>
                <groupId>org.hibernate.validator</groupId>
                <artifactId>hibernate-validator</artifactId>
                <version>7.0.1.Final</version>
            </dependency>
            <!-- mybatis引用 -->
            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.2.0</version>
            </dependency>
            <!-- tk.mybatis引用 -->
            <dependency>
                <groupId>tk.mybatis</groupId>
                <artifactId>mapper-spring-boot-starter</artifactId>
                <version>2.1.5</version>
            </dependency>
            <!-- 热部署 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-devtools</artifactId>
                <version>2.5.4</version>
                <scope>runtime</scope>
                <optional>true</optional>
            </dependency>
            <!-- mysql依赖 -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.26</version>
                <scope>runtime</scope>
            </dependency>
            <!-- 分页查询 -->
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper</artifactId>
                <version>5.2.1</version>
            </dependency>
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-autoconfigure</artifactId>
                <version>1.3.0</version>
            </dependency>
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>1.3.0</version>
            </dependency>
            <!-- lombok简化开发 -->
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.18.20</version>
                <optional>true</optional>
            </dependency>
            <!-- 工具类 -->
            <dependency>
                <groupId>org.apache.commons</groupId>
                <artifactId>commons-lang3</artifactId>
                <version>3.12.0</version>
            </dependency>
            <!-- beanutils -->
            <dependency>
                <groupId>commons-beanutils</groupId>
                <artifactId>commons-beanutils</artifactId>
                <version>1.9.4</version>
            </dependency>
            <!-- excel工具 -->
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi</artifactId>
                <version>3.17</version>
            </dependency>
            <dependency>
                <groupId>org.apache.poi</groupId>
                <artifactId>poi-ooxml</artifactId>
                <version>3.17</version>
            </dependency>
            <dependency>
                <groupId>xerces</groupId>
                <artifactId>xercesImpl</artifactId>
                <version>2.12.0</version>
            </dependency>
            <!-- spring-test引用 -->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <version>2.5.4</version>
                <scope>test</scope>
                <exclusions>
                    <exclusion>
                        <groupId>org.junit.vintage</groupId>
                        <artifactId>junit-vintage-engine</artifactId>
                    </exclusion>
                </exclusions>
            </dependency>
            <!-- knife4j 接口文档 -->
            <dependency>
                <groupId>com.github.xiaoymin</groupId>
                <artifactId>knife4j-spring-boot-starter</artifactId>
                <version>2.0.7</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.5.4</version>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```



### 附录

maven提供的41个骨架原型分别是：

1. internal -> appfuse-basic-jsf (创建一个基于Hibernate，Spring和JSF的Web应用程序的原型) 
2. internal -> appfuse-basic-spring (创建一个基于Hibernate，Spring和Spring MVC的Web应用程序的原型) 
3. internal -> appfuse-basic-struts (创建一个基于Hibernate，Spring和Struts 2的Web应用程序的原型) 
4. internal -> appfuse-basic-tapestry (创建一个基于Hibernate, Spring 和 Tapestry 4的Web应用程序的原型) 
5. internal -> appfuse-core (创建一个基于 Hibernate and Spring 和 XFire的jar应用程序的原型) 
6. internal -> appfuse-modular-jsf (创建一个基于 Hibernate，Spring和JSF的模块化应用原型) 
7. internal -> appfuse-modular-spring (创建一个基于 Hibernate, Spring 和 Spring MVC 的模块化应用原型) 
8. internal -> appfuse-modular-struts (创建一个基于 Hibernate, Spring 和 Struts 2 的模块化应用原型) 
9. internal -> appfuse-modular-tapestry (创建一个基于 Hibernate, Spring 和 Tapestry 4 的模块化应用原型) 
10. internal -> maven-archetype-j2ee-simple (一个简单的J2EE的Java应用程序) 
11. internal -> maven-archetype-marmalade-mojo (一个Maven的 插件开发项目 using marmalade) 
12. internal -> maven-archetype-mojo (一个Maven的Java插件开发项目) 
13. internal -> maven-archetype-portlet (一个简单的portlet应用程序) 
14. internal -> maven-archetype-profiles () 
15. internal -> maven-archetype-quickstart () 
16. internal -> maven-archetype-site-simple (简单的网站生成项目) 
17. internal -> maven-archetype-site (更复杂的网站项目) 
18. internal -> maven-archetype-webapp (一个简单的Java Web应用程序) 
19. internal -> jini-service-archetype (Archetype for Jini service project creation) 
20. internal -> softeu-archetype-seam (JSF+Facelets+Seam Archetype) 
21. internal -> softeu-archetype-seam-simple (JSF+Facelets+Seam (无残留) 原型) 
22. internal -> softeu-archetype-jsf (JSF+Facelets 原型) 
23. internal -> jpa-maven-archetype (JPA 应用程序) 
24. internal -> spring-osgi-bundle-archetype (Spring-OSGi 原型) 
25. internal -> confluence-plugin-archetype (Atlassian 聚合插件原型) 
26. internal -> jira-plugin-archetype (Atlassian JIRA 插件原型) 
27. internal -> maven-archetype-har (Hibernate 存档) 
28. internal -> maven-archetype-sar (JBoss 服务存档) 
29. internal -> wicket-archetype-quickstart (一个简单的Apache Wicket的项目) 
30. internal -> scala-archetype-simple (一个简单的scala的项目) 
31. internal -> lift-archetype-blank (一个 blank/empty liftweb 项目) 
32. internal -> lift-archetype-basic (基本（liftweb）项目) 
33. internal -> cocoon-22-archetype-block-plain ([http://cocoapacorg2/maven-plugins/]) 
34. internal -> cocoon-22-archetype-block ([http://cocoapacorg2/maven-plugins/]) 
35. internal -> cocoon-22-archetype-webapp ([http://cocoapacorg2/maven-plugins/]) 
36. internal -> myfaces-archetype-helloworld (使用MyFaces的一个简单的原型) 
37. internal -> myfaces-archetype-helloworld-facelets (一个使用MyFaces和Facelets的简单原型) 
38. internal -> myfaces-archetype-trinidad (一个使用MyFaces和Trinidad的简单原型) 
39. internal -> myfaces-archetype-jsfcomponents (一种使用MyFaces创建定制JSF组件的简单的原型) 
40. internal -> gmaven-archetype-basic (Groovy的基本原型) 
41. internal -> gmaven-archetype-mojo (Groovy mojo 原型)

