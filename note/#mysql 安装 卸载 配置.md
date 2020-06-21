### 下载

下载地址：

```
https://dev.mysql.com/downloads/mysql/

https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/

https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.26-winx64.zip
```



### 安装

解压后进入文件夹，创建`my.ini`文件，粘贴以下内容：

```ini
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
default-time-zone='+08:00'
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=D:\\App\\mysql-8.0.16-winx64
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=C:\\web\\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
#character-set-server=utf8mb4  mysql 8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
注意需要修改目录路径。

使用管理员权限进入`bin`目录，初始化数据库：
```bash
mysqld --initialize --user=mysql --console
```

初始化完成后会显示一个随机生成的密码，记下来。

接下来执行安装命令：
```bash
mysqld --install servicename --defaults-file="D:\software\mysql\my.ini" 
```

可以在`servicename`这里自定义服务名，建议默认就用mysql

然后启动：

```bash
net start mysql
```

登录：
```bash
mysql -h 主机名 -u 用户名 -p
or
mysql -u 用户名(root) -p
or
mysql -h 127.0.0.1 -uroot -p -P 端口号
```

登录进mysql后，修改密码：
```bash
# mysql 8 版本不适用password命令
set password=password("123456");

alter user 'root'@'localhost' identified by '设置的新密码'
```

---
### 主从复制

在同一台机器上测试主从复制。

假设3307为主，3308为从。

主配置：

```ini
[client]
port=3307
default-character-set=utf8

[mysqld] 
#主库配置
server_id=1
log_bin=master-bin
log_bin-index=master-bin.index

# 设置为自己MYSQL的安装目录
basedir=D:/Work/DB/mysql-replication/master-3307
# 设置为MYSQL的数据目录，data文件夹由mysql自动生成
datadir=D:/Work/DB/mysql-replication/master-3307/data
port=3307
character_set_server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER

# 开启查询缓存
explicit_defaults_for_timestamp=true

```



从配置：

```ini
[client]
port=3308
default-character-set=utf8

[mysqld] 
#从库配置
server_id=2
relay-log-index=slave-relay-bin.index
relay-log=slave-relay-bin

# 设置为自己MYSQL的安装目录
basedir=D:/Work/DB/mysql-replication/follower-3308
# 设置为MYSQL的数据目录，data文件夹由mysql自动生成
datadir=D:/Work/DB/mysql-replication/follower-3308/data
port=3308
character_set_server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,NO_AUTO_CREATE_USER

# 开启查询缓存
explicit_defaults_for_timestamp=true

```



按照安装部分的步骤将主从两份mysql都安装好以后，登录进入主库：

```sql
show master status;

#返回结果示例：
+-------------------+----------+--------------+------------------+-------------------+
| File              | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+-------------------+----------+--------------+------------------+-------------------+
| master-bin.000002 |      398 |              |                  |                   |
+-------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

相应的在主库的data目录下也生成了对应的文件。



然后登录从库，并执行：

```sql
change master to master_host='127.0.0.1',master_port=3307,master_user='root',master_password='root',master_log_file='master-bin.000002',master_log_pos=0;
```

执行完成后，去主库测试，建库，建表，写数据，然后观察从库是否有相应的同步数据。

因为两个库只是做了单向关联，如果往从库中写数据的话，主库是无法同步的。所以从库只能用于读取数据，而主库既能写，也能读，当然，多数情况都是用于写数据，读取数据一般都是从库获取，这样能有效减轻主库的压力，也就是我们常说的读写分离。

参考：https://juejin.im/post/5dd13778e51d453da86c0e6f



---



### 卸载

**停止mysql服务**

```bash
net stop mysql
```
或者打开任务管理器，服务tab，找到mysql停止。

**删除服务**

```bash
sc delete mysql
```

然后删除整个mysql文件夹

**删除注册表**
打开regedit

```
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Applications/MySQL
```



---

### 修改mysql 服务 的启动路径

打开regedit
```
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Applications/MySQL
```
上述位置找到MYSQL，然后修改imagePath即可。

