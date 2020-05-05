### 服务自启动

centos 6 ：使用chkconfig命令即可。

我们以apache服务为例：

```
#chkconfig --add apache 添加nginx服务

#chkconfig apache on 开机自启nginx服务

#chkconfig apache off 关闭开机自启

#chkconfig --list | grep apache 查看
```

---

centos 7 ：使用systemctl中的enable、disable 即可。
示例：
```bash
#systemctl enable apache.service 开机自启apache服务

#systemctl disable apache.service 关闭开机自启
```



### tar,解压

例如解压`apache-zookeeper-3.6.0-bin.tar.gz`文件:

```bash
tar -xzvf apache-zookeeper-3.6.0-bin.tar.gz
```



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



### 设置dns

```
sudo vim /etc/systemd/resolved.conf
```

```bash
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.
#
# Entries in this file show the compile time defaults.
# You can change settings by editing this file.
# Defaults can be restored by simply deleting this file.
#
# See resolved.conf(5) for details

[Resolve]
DNS=8.8.8.8
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#Cache=yes
#DNSStubListener=yes

```

2. 重启服务

```bash
systemctl restart systemd-resolved
```

3. 设置服务自启动

```bash
systemctl enable systemd-resolved
```

