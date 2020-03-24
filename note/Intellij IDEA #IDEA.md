### Intellij IDEA运行报Command line is too long
修改项目下 .idea\workspace.xml，找到标签 
```xml
<component name="PropertiesComponent">
```
 在标签里加一行  
```xml
<property name="dynamic.classpath" value="true" />
```



---

### 一个项目启动多个实例

菜单栏`Run`-`Edit Configurations`，复制一份，然后修改`Program arguments`即可。

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-24_13-58-11.jpg)

