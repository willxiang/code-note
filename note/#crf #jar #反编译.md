### CFR(Class File Reader)

下载地址：

https://github.com/leibnitz27/cfr

https://mvnrepository.com/artifact/org.benf/cfr

https://www.benf.org/other/cfr/



下载下来是一个jar包。



#### 反编译jar包

```bash
#将target.jar包反编译到当前目录下的targetDir文件夹
java -jar cfr-0.149.jar target.jar --outputdir targetDir
```

#### 反编译Class文件

```bash
java -jar cfr-0.149.jar JavaTest.class
```



#### 包装成脚本

##### 反编译某个class文件

```bash
java -jar ~/Documents/scripts/cfr-0.139.jar $1
```

##### 反编译某个jar包

```bash
java -jar ~/Documents/scripts/cfr-0.139.jar $1 --outputdir $2
```



> 参考资料：https://droidyue.com/blog/2019/02/24/decompile-class-file-command-line/

