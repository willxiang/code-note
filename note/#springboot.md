[TOC]

### 开启文件上传功能

```yaml
spring:
  servlet:
    multipart:
      enabled:true#开启文件上传
      max-file-size:10MB#限制文件上传大小为10M
```



---



### springboot Linux shell 脚本 部署

start.sh：

```
#!/bin/bash
APPNAME=yourapp.jar
nohup java -jar $APPNAME &
echo $APPNAME is running
```



stop.sh:

```
#!/bin/bash
APPNAME=yourapp.jar
PID=$(ps -ef | grep $APPNAME | grep -v grep | awk '{ print $2 }')
if [ -z "$PID" ]
then
    echo $APPNAME is already stopped
else
    echo kill $PID
    kill $PID
fi
```



run.sh:

```
#!/bin/bash
echo stop yourapp
source stop.sh
echo start yourapp
source start.sh
```



运行脚本时获取管理员权限：

```
chmod +x run.sh
```



脚本如果运行报错，可尝试：

```
yum -y install dos2unix

dos2unix run.sh

sh run.sh
```

