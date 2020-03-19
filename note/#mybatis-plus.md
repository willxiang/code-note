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
```java
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

---

### 配置mapper package扫描路径

直接在Application.java类中注解配置：

```java
@MapperScan("com.baomidou.mybatisplus.samples.crud.mapper")
```

或单独新增一个配置类，在配置类上增加注解：

```java
@Configuration
@MapperScan("com.baomidou.mybatisplus.samples.crud.mapper")
public class MybatisPlusConfig {

}
```

