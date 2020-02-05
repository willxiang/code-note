### CentOS 7+ 开启端口

在指定区域开启端口(如80端口号，命令方式)

```
firewall-cmd --zone=public --add-port=80/tcp --permanent
```

解释：

- zone 作用域
- add-port=80/tcp 添加端口，格式为：端口/通讯协议
- permanent，永久生效，不加该参数则重启后失效


查看是否开启某端口
```
firewall-cmd --query-port=80/tcp
```



重启防火墙

```
firewall-cmd --reload
```




---
问题：nginx代理8081端口无效，查看日志后发现[emerg] bind() to 0.0.0.0:8081 failed (13: Permission denied)
解决：
首先，查看http允许访问的端口：
```
semanage port -l | grep http_port_t
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
```
其次，将要启动的端口加入到如上端口列表中
```
semanage port -a -t http_port_t  -p tcp 8081
```
---
问题：SElinux error :ValueError: Port tcp/8081 already defined
解决：
yum安装semanage
```
yum install semanage
或者
yum provides semanage
```
```
yum -y install policycoreutils-python.x86_64
```

```
semanage port -m -t http_port_t -p tcp 8081
```
然后重启nginx。
https://serverfault.com/questions/790404/selinux-error-valueerror-port-tcp-5000-already-defined
