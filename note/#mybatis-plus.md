### insert 数据，按id自增
直接修改配置文件，此配置全局生效：
```
mybatis-plus.global-config.db-config.id-type=auto
```
或者单独对某个实体的主键配置：
```
@TableId(type = IdType.AUTO)
private Long id;
```
注意：建表时需要主键`AUTO_INCREMENT`。
若不配置，则会按照mybatis-plus的全局唯一id生成数据。

---

### 关闭驼峰策略（实体字段）
```
mybatis-plus.configuration.map-underscore-to-camel-case=false
```
---

### 打印SQL

```
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

---

### 分页配置
```
@Configuration
@EnableTransactionManagement
public class MyBatisPlusConfig {

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor interceptor = new PaginationInterceptor();
        interceptor.setDialectType("mysql");
        return interceptor;
    }
}
```