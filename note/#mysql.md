### Unable to load authentication plugin 'caching_sha2_password'.


>Starting with MySQL 8.0.4, they have changed the default authentication plugin for MySQL server from **mysql_native_password** to **caching_sha2_password**.

https://dev.mysql.com/doc/refman/8.0/en/caching-sha2-pluggable-authentication.html

解决方法：

```
ALTER USER '账号'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码';
```



### dump 导出数据

```bash
mysqldump -u root -p dbname > d:\dump.sql
```

该命令会将指定数据库导出`sql`格式文本。



### Too many connections

先查看一下最大连接数：

```sql
show variables like '%max_connections%';
```

一般默认未修改的话是`20`，

如果过于小，则设置大一点：

```sql
set GLOBAL max_connections=100;
```





### sql_mode=only_full_group_by

查看sql_model参数命令：

```
SELECT @@GLOBAL.sql_mode;

SELECT @@SESSION.sql_mode;
```

结果是否如下：
**ONLY_FULL_GROUP_BY**,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

解决方案：
```sql
set sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';

--如果上面设置完成后依然报错，这里执行下面的SQL，将全局设置更改，然后必须关闭现有的连接，重新打开连接后查询即可
 
set global sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```





### The user specified as a definer ('root'@'%') does not exist

```sql
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;

FLUSH PRIVILEGES; 
```

