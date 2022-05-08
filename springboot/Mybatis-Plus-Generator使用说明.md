## Mybatis-Plus Generator依赖与配置

**依赖**

``` java
<!-- mysql数据源 -->
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>
<!-- mybatis 依赖 -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>${mybatis.version}</version>
</dependency>
<!-- mybatis-plus 依赖 -->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>${mybatis-plus.version}</version>
</dependency>
<!-- mybatis-plus-generator 依赖 -->
<dependency>
	<groupId>com.baomidou</groupId>
	<artifactId>mybatis-plus-generator</artifactId>
	<version>${mybatis-plus.version}</version>
</dependency>
<!-- springboot freemarker 依赖 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

mybatis-plus 相关注解及用法详见 [Mybatis-Plus官网]([简介 | MyBatis-Plus (baomidou.com)](https://mp.baomidou.com/guide/))



**代码生成工具配置**

``` java
public class MybatisPlusGenerator {

    /**
     * <p>
     * 读取控制台内容
     * </p>
     */
    public static String scanner(String tip) {
        Scanner scanner = new Scanner(System.in);
        StringBuilder help = new StringBuilder();
        help.append("请输入" + tip + "：");
        System.out.println(help.toString());
        if (scanner.hasNext()) {
            String ipt = scanner.next();
            if (StringUtils.isNotBlank(ipt)) {
                return ipt;
            }
        }
        throw new MybatisPlusException("请输入正确的" + tip + "！");
    }

    public static void main(String[] args) {
        String projectPath = System.getProperty("user.dir") + "/cygc-modules";
        String moduleName = scanner("模块名");
        String prefix = scanner("表前缀") + "_";
        String tables = scanner("表名，多个英文逗号分割");
        // 代码生成器
        AutoGenerator mpg = new AutoGenerator();
        // 数据源配置
        mpg.setDataSource(getDataSourceConfig());
        // 包配置
        mpg.setPackageInfo(getPackageConfig(moduleName));
        // 全局配置
        mpg.setGlobalConfig(getGlobalConfig(projectPath));
        // 策略配置
        mpg.setStrategy(getStrategyConfig(prefix, tables));
        // 自定义配置
        mpg.setCfg(getInjectionConfig(projectPath, moduleName));
        // 配置模板
        mpg.setTemplate(getTemplateConfig());
        mpg.setTemplateEngine(new FreemarkerTemplateEngine());
        mpg.execute();
    }

    /**
     * 数据源配置
     *
     * @return
     */
    private static DataSourceConfig getDataSourceConfig() {
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setUrl("jdbc:mysql://172.18.101.161:3306/cygc?serverTimezone=Asia/Shanghai&characterEncoding=utf8&useSSL=false");
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUsername("root");
        dsc.setPassword("cqcca@Mysql");
        return dsc;
    }

    /**
     * 包配置
     *
     * @return
     */
    private static PackageConfig getPackageConfig(String moduleName) {
        PackageConfig pc = new PackageConfig();
        pc.setModuleName(moduleName);
        pc.setParent("com.cqcca.cygc.modules");
        return pc;
    }

    /**
     * 全局策略配置
     *
     * @param projectPath
     * @return
     */
    private static GlobalConfig getGlobalConfig(String projectPath) {
        GlobalConfig gc = new GlobalConfig();
        gc.setOutputDir(projectPath + "/src/main/java");
        gc.setAuthor("cqcca");
        gc.setOpen(false);
        // 自定义文件名
        gc.setMapperName("%sMapper");
        gc.setXmlName("%sMapper");
        gc.setControllerName("%sController");
        gc.setServiceName("%sService");
        gc.setServiceImplName("%sServiceImpl");
        gc.setIdType(IdType.ASSIGN_UUID);
        gc.setBaseResultMap(true);
        gc.setBaseColumnList(true);
        gc.setDateType(DateType.ONLY_DATE);
        // gc.setSwagger2(true);
        return gc;
    }

    /**
     * 策略配置
     *
     * @return
     */
    private static StrategyConfig getStrategyConfig(String prefix, String tables) {
        StrategyConfig strategy = new StrategyConfig();
        strategy.setTablePrefix(prefix);
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
        strategy.setChainModel(true);
        strategy.setSuperEntityClass(BaseEntity.class);
        strategy.setSuperEntityColumns(new String[]{"created_by", "create_time", "updated_by", "update_time"});
        strategy.setEntityTableFieldAnnotationEnable(true);
        strategy.setEntityLombokModel(true);
        strategy.setRestControllerStyle(true);
        strategy.setInclude(tables.split(","));
        strategy.setControllerMappingHyphenStyle(true);
        List<TableFill> tableFills = new ArrayList<>();
        tableFills.add(new TableFill("create_time", FieldFill.INSERT));
        tableFills.add(new TableFill("update_time", FieldFill.INSERT_UPDATE));
        strategy.setTableFillList(tableFills);
        return strategy;
    }

    /**
     * 自定义配置
     *
     * @param projectPath
     * @return
     */
    private static InjectionConfig getInjectionConfig(String projectPath, String moduleName) {
        InjectionConfig cfg = new InjectionConfig() {
            @Override
            public void initMap() {
                // to do nothing
            }
        };
        // 如果模板引擎是 freemarker
        String templatePath = "/template/mapper.xml.ftl";
        // 如果模板引擎是 velocity
        // String templatePath = "/templates/mapper.xml.vm";
        // 自定义输出配置
        List<FileOutConfig> focList = new ArrayList<>();
        // 自定义配置会被优先输出
        focList.add(new FileOutConfig(templatePath) {
            @Override
            public String outputFile(TableInfo tableInfo) {
                // 自定义输出文件名 ， 如果你 Entity 设置了前后缀、此处注意 xml 的名称会跟着发生变化！！
                return projectPath + "/src/main/resources/mapper/" + moduleName
                        + "/" + tableInfo.getEntityName() + "Mapper" + StringPool.DOT_XML;
            }
        });
        cfg.setFileOutConfigList(focList);
        return cfg;
    }

    /**
     * 模板配置
     *
     * @return
     */
    private static TemplateConfig getTemplateConfig() {
        TemplateConfig templateConfig = new TemplateConfig();
        templateConfig.setXml(null);
        return templateConfig;
    }
}
```

