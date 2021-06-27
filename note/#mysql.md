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



### 按时间分组统计

```sql
<!-- 按日查询 -->  
SELECT DATE_FORMAT(created_date,'%Y-%m-%d') as time,sum(money) money FROM o_finance_detail where org_id = 1000  GROUP BY  time  
<!-- 按月查询 -->  
SELECT DATE_FORMAT(created_date,'%Y-%m') as time,sum(money)  money FROM o_finance_detail where org_id = 1000  GROUP BY  time  
<!-- 按年查询 -->  
SELECT DATE_FORMAT(created_date,'%Y') as time,sum(money)  money FROM o_finance_detail where org_id = 1000  GROUP BY  time   
<!-- 按周查询 -->  
SELECT DATE_FORMAT(created_date,'%Y-%u') as time,sum(money)  money FROM o_finance_detail where org_id = 1000  GROUP BY  time


DATE_FORMAT(date,format) 
根据format字符串格式化date值。下列修饰符可以被用在format字符串中： 
%M 月名字(January……December) 
%W 星期名字(Sunday……Saturday) 
%D 有英语前缀的月份的日期(1st, 2nd, 3rd, 等等。） 
%Y 年, 数字, 4 位 
%y 年, 数字, 2 位 
%a 缩写的星期名字(Sun……Sat) 
%d 月份中的天数, 数字(00……31) 
%e 月份中的天数, 数字(0……31) 
%m 月, 数字(01……12) 
%c 月, 数字(1……12) 
%b 缩写的月份名字(Jan……Dec) 
%j 一年中的天数(001……366) 
%H 小时(00……23) 
%k 小时(0……23) 
%h 小时(01……12) 
%I 小时(01……12) 
%l 小时(1……12) 
%i 分钟, 数字(00……59) 
%r 时间,12 小时(hh:mm:ss [AP]M) 
%T 时间,24 小时(hh:mm:ss) 
%S 秒(00……59) 
%s 秒(00……59) 
%p AM或PM 
%w 一个星期中的天数(0=Sunday ……6=Saturday ） 
%U 星期(0……52), 这里星期天是星期的第一天 
%u 星期(0……52), 这里星期一是星期的第一天 
%% 一个文字“%”。
```



