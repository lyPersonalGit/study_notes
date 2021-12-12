# Knife4j的依赖与配置

依赖

``` java
<!-- knife4j 接口文档 -->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>${knife4j.version}</version>
</dependency>
```

配置举例

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
                        .description("吉林省危险化学品罐车动态管理平台接口文档")
                        .termsOfServiceUrl("https://www.xx.com/")
                        .contact(new Contact("author", "index", "xx@qq.com"))
                        .version("1.0")
                        .build())
                //分组名称
                .groupName("系统管理模块")
                .select()
                //这里指定Controller扫描包路径
				.apis(RequestHandlerSelectors.basePackage("com.cqcca.cygc.modules.system.controller"))
                .paths(PathSelectors.any())
                .build();
        return docket;
    }
}
```

