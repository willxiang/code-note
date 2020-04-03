### 安装node.js

[下载安装程序（LTS版本）](https://nodejs.org/en/download/) 安装即可。



### 安装hexo

```bash
npm install -g hexo-cli
```



### 生成博客

安装完成后，选择一个文件夹当作你的博客目录，比如：

```bash
D:\Blog
```

进入这个目录，使用`Hexo`初始化博客：

```bash
hexo init
```



### 启动博客

完毕后，启动本地`Hexo`查看刚刚生成的博客：

```bash
hexo s
```

启动成功后访问本地端口4000页面，即可看到界面。



### 部署到Github

#### 创建Github对应仓库

假设你的github帐号是：`zhangsan`，那么就去创建一个名为`zhangsan.github.io`的仓库。



#### 修改 _config.yml

博客文件夹根目录有一个`_config.yml`文件，打开后查看`# Deployment`（最下方），修改部署的信息：

```yml
deploy:
  type: git
  repo: git@github.com:zhangsan/zhangsan.github.io.git
  branch: master
```



#### 安装部署插件

```bash
npm install hexo-deployer-git --save
```

然后分别输入以下命令：

```bash
hexo clean
hexo g      # 生成
hexo d      # 部署
```

重新生成后会自动部署并推送到github的对应仓库中，操作成功后，稍等片刻，即可访问你仓库名的地址直接查砍博客了。



#### 更换主题

以`https://github.com/litten/hexo-theme-yilia`为例，clone项目到博客的`themes`目录：

```bash
git clone git@github.com:litten/hexo-theme-yilia.git themes/yilia
```

clone完毕后，会在`themes`目录下生成一个叫做`yilia`的文件夹，这就是一份主题。

随后修改`_config.yml`：

```yml
theme: yilia
```

找到`theme`关键字，修改默认的`landscape`为`yilia`，重新生成部署即可。



### 发布文章

```bash
hexo n "博客名字"
```

执行命令后会在`source\_posts`目录下生成一个`博客名字.md`文件。