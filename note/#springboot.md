[TOC]

### dev热部署

子模块pom.xml

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
</dependency>
```

父工程pom.xml

```xml
<build>
<!--        <finalName></finalName>-->
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <addResources>true</addResources>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

IDEA开启自动编译功能

​	![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-04-24_13-17-02.jpg)

`Ctrl+Shift+Alt+/`：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-04-24_13-20-07.jpg)

`Registry`：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-04-24_13-27-01.jpg)



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





### 参数校验



```xml
        <!--jsr 303-->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>1.1.0.Final</version>
        </dependency>
        <!-- hibernate validator-->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.2.0.Final</version>
        </dependency>
```



```
   @NotNull：不能为null，但可以为empty(""," ","   ")      
   @NotEmpty：不能为null，而且长度必须大于0 (" ","  ")
   @NotBlank：只能作用在String上，不能为null，而且调用trim()后，长度必须大于0("test")    即：必须有实际字符
```



```java
//控制器 添加 @Validated
//@ControllerAdvice 全局异常处理，要针对 MethodArgumentNotValidException、ConstraintViolationException 做处理。

    public DataResult<List<OrgInfoVO>> orgList(@NotNull(message = "model 不能为空") Integer model, @ApiIgnore User user) {
        return DataResult.success(orgService.getOrgListByModel(user.getOrgId(), model));
    }
```