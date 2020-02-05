# winsw
https://github.com/kohsuke/winsw



##### XML配置文件
```xml
  <configuration>
  <!-- 系统服务ID，不能与其他系统服务名称重复-->
  <id>springboot</id>
  <!-- 系统服务显示名称 -->
  <name>springboot (powered by WinSW)</name>
  <!-- 服务描述 -->
  <description>springboot Service</description>
  <!-- java环境变量 -->
  <env name="JAVA_HOME" value="%JAVA_HOME%"/>
  <!-- 需要执行的应用 命令 -->
  <executable>java</executable>  
  <!-- 命令参数，分行可以用多行'argument'标记 -->
  <arguments>-jar springboot.jar</arguments>
</configuration>
```

##### 各个文件准备

- springboot.jar
- springboot-service.exe
- springboot-service.xml

将要执行的jar包，winsw.exe改名为springboot-service.exe（名字可自定义），xml文件跟exe文件同名，以上三个文件放在一个目录。



##### 部署

使用管理员权限打开cmd进入到上述三个文件的目录，执行下面的命令，安装成服务：

```bash
springboot-service.exe install
```

启动服务：

```bash
net start springboot
```



---

常用命令：

- install	将服务安装到windows服务器
- uninstall	卸载服务
- start	开始服务
- stop	停止服务
- restart	重启服务
- status	查看服务状态
  - NonExistent	该服务尚未安装
  - Started	服务正在运行
  - Stopped	服务已停止


---
```
@echo off
start javaw -jar ksw-web.jar
echo 程序正在启动，请等待1-3分钟后访问……
pause
```

```
@echo off
set port=9158
for /f "tokens=1-5" %%i in ('netstat -ano^|findstr ":%port%"') do taskkill /f /pid %%m
```