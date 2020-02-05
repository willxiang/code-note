下载地址：
```
https://dev.mysql.com/downloads/mysql/

https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/

https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQL-5.7/mysql-5.7.26-winx64.zip
```

解压后进入文件夹，创建`my.ini`文件，粘贴以下内容：
```
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
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
注意需要修改目录路径。

进入`bin`目录，初始化数据库：
```
mysqld --initialize --console
```

初始化完成后会显示一个随机生成的密码，记下来。

接下来执行安装命令：
```
mysqld install
```

然后启动：
```
net start mysql
```

登录：
```
mysql -h 主机名 -u 用户名 -p
or
mysql -u 用户名(root) -p
```

修改密码：
```
ALTER USER 'userName'@'localhost' IDENTIFIED BY 'New-Password-Here';
```

---
### 卸载 mysql


**停止mysql服务**
```
net stop mysql
```
或者打开任务管理器，服务tab，找到mysql停止。

**卸载mysql服务**
```
mysqld -remove
```
然后删除整个mysql文件夹

**删除注册表**
打开regedit
```
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Applications/MySQL
```


### 修改mysql 服务 的启动路径
打开regedit，
```
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet001/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/ControlSet002/Services/Eventlog/Applications/MySQL 
HKEY_LOCAL_MACHINE/SYSTEM/CurrentControlSet/Services/Eventlog/Applications/MySQL
```
上述位置找到MYSQL，然后修改imagePath即可。