# Knife4j的依赖与配置

pom依赖

``` xml
<!-- https://mvnrepository.com/artifact/com.github.xiaoymin/knife4j-spring-boot-starter -->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>${knife4j.version}</version>
</dependency>
```

配置举例，当功能分布在多个模块下时，每个docket对应一个功能模块

``` java
@Configuration
@EnableSwagger2WebMvc
public class Knife4jConfig {

    private final static String SPLIT_SYMBOL = ";";

    @Bean(value = "systemApi")
    public Docket systemApi() {
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(new ApiInfoBuilder()
                        //.title("swagger-bootstrap-ui-demo RESTFUL APIs")
                        .description("测试项目接口文档")
                        .termsOfServiceUrl("https://www.xx.com/")
                        .contact(new Contact("author", "index", "xx@qq.com"))
                        .version("1.0")
                        .build())
                //分组名称
                .groupName("系统管理模块")
                .select()
                //这里指定Controller扫描包路径
				.apis(RequestHandlerSelectors.basePackage("com.example.project.modules.system.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
    
    @Bean(value = "securityApi")
    public Docket securityApi() {
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(new ApiInfoBuilder()
                        //.title("swagger-bootstrap-ui-demo RESTFUL APIs")
                        .description("测试项目接口文档")
                        .termsOfServiceUrl("https://www.xx.com/")
                        .contact(new Contact("author", "index", "xx@qq.com"))
                        .version("1.0")
                        .build())
                //分组名称
                .groupName("安全认证模块")
                .select()
                //这里指定Controller扫描包路径
                .apis(RequestHandlerSelectors.basePackage("com.example.project.modules.security.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
}
```

