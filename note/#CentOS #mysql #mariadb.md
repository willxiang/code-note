mariadb无法启动，查看日志：
```
190811 07:39:24 mysqld_safe mysqld from pid file /var/run/mariadb/mariadb.pid ended
190811 09:39:27 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
190811  9:39:27 [Note] /usr/libexec/mysqld (mysqld 5.5.60-MariaDB) starting as process 1398 ...
190811  9:39:27 InnoDB: The InnoDB memory heap is disabled
190811  9:39:27 InnoDB: Mutexes and rw_locks use GCC atomic builtins
190811  9:39:27 InnoDB: Compressed tables use zlib 1.2.7
190811  9:39:27 InnoDB: Using Linux native AIO
190811  9:39:27 InnoDB: Initializing buffer pool, size = 128.0M
190811  9:39:27 InnoDB: Completed initialization of buffer pool
190811  9:39:27 InnoDB: highest supported file format is Barracuda.
190811  9:39:27  InnoDB: Waiting for the background threads to start
190811  9:39:28 Percona XtraDB (http://www.percona.com) 5.5.59-MariaDB-38.11 started; log sequence number 1597945
190811  9:39:28 [Note] Plugin 'FEEDBACK' is disabled.
190811  9:39:28 [Note] Server socket created on IP: '0.0.0.0'.
190811  9:39:28 [ERROR] Can't start server: Bind on TCP/IP port. Got error: 13: Permission denied
190811  9:39:28 [ERROR] Do you already have another mysqld server running on port: 3306 ?
190811  9:39:28 [ERROR] Aborting

190811  9:39:28  InnoDB: Starting shutdown...
190811  9:39:32  InnoDB: Shutdown completed; log sequence number 1597945
190811  9:39:32 [Note] /usr/libexec/mysqld: Shutdown complete
```
看似好像是3306端口被占用，于是查看端口：
```
netstat -apn  | grep 3306
```
发现3306端口并未被使用。

一顿搜索后：
```
vi /etc/selinux/config
```
将:
```
SELINUX=disabled
```
重启虚拟机后mariadb正常启动了。

然后物理机SQL GUI访问虚拟机数据库时，
```
190811  9:47:02 [Note] Hostname 'DESKTOP-XXXX' does not resolve to '192.168.137.1'.
190811  9:47:02 [Note] Hostname 'DESKTOP-XXXX' has the following IP addresses:
```
于是虚拟机连接数据库，
```
mysql -u root -p

use mysql;

select user,host from user;
>
+------+---------------+
| user | host          |
+------+---------------+
| root | 127.0.0.1     |
| root | 192.168.34.44 |
| root | ::1           |
| root | localhost     |
+------+---------------+
4 rows in set (0.00 sec)

```

于是执行语句：
```
grant all privileges on *.* to  'root'@"192.168.137.1" identified by "root" with grant option;

或者

grant all privileges on *.* to  'root'@"%" identified by "root" with grant option;
```
即可。
---

### mariadb 安装
<img src="https://raw.githubusercontent.com/willxiang/code-note/master/img/screencapture-kclouder-cn-cenos7-mariadb-2019-08-11-23_33_15.png"/>
