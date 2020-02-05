### maven 打包报错

```
maven.plugin.compiler.CompilationFailureException: Compilation failure
```

检查`pom.xml`并添加：
```
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```
在打包命令后边加上`-e`参数再打包，查看具体报错内容所在文件的代码行数，再根据实际情况解决。
