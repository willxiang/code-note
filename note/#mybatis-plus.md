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

### 分页

配置：
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

service实现类中可以直接使用`com.baomidou.mybatisplus.core.mapper.BaseMapper`的`selectPage`方法：
```java
@service 
public IPage getGoodsPage(Page page, Goods goods) {
        QueryWrapper<Goods> queryWrapper = new QueryWrapper<>();
        queryWrapper.like("goods_name", goods.getGoodsName());
        return baseMapper.selectPage(page, queryWrapper);
    }

@controller 
@GetMapping("/page")
    public Object getGoodsPage(Page page, Goods goods) {
        return goodsService.getGoodsPage(page, goods);
    }
```

此方法返回`Page`对象，该对象包含：

```java
	private List<T> records;
    private long total;
    private long size;
    private long current;
    private List<OrderItem> orders;
    private boolean optimizeCountSql;
    private boolean isSearchCount;
    private boolean hitCount;
```

其中常见常用的分页属性有：total（数据总数），size（一页显示多少条数），current（当前是第几页）。

前端查询时传入current，size即可实现分页。



![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-23_20-46-31.jpg)

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

