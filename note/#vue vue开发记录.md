# Vue 开发记录



## 安装Vue CLI

https://cli.vuejs.org/zh/guide/installation.html

```
npm install -g @vue/cli
```



## 创建项目

##### 使用界面创建项目

```
vue ui
```

打开图形化界面后，创建新项目，填写名称，接下来的步骤开始使用截图记录一下：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-04-30.jpg)

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-15-22.jpg)

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-17-15.jpg)

接下来创建项目，耐心等待下载依赖。项目创建完成后的目录结构：
![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-23-02.jpg)

解释一下目录结构：

- src
  - assets: 存放css,fonts,图片等资源
  - components/views: 之后vue的单文件（主要的页面文件）可以按照不同功能存放在这个文件夹下
  - router: 文件夹下的index.js是vue-router的配置内容
  - main.js 项目的打包入口文件
  - .eslintrc.js: eslint的配置文件
  - babel.config.js: babel配置文件

可在根目录下创建`vue.config.js`文件，用来进行一些自定义的配置，具体可参考[vue.config.js](https://cli.vuejs.org/zh/config/#vue-config-js)

例如，修改启动端口号可以这样[配置](https://cli.vuejs.org/zh/config/#devserver)：

```json
module.exports = {
  devServer: {
    open: true,
    port: 8888
  }
}
```

关于`devServer`的配置，扩展阅读：https://webpack.js.org/configuration/dev-server

运行以下命令即可启动刚刚创建的项目

```
npm run serve
```

##### 安装 element 插件
![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-54-19.jpg)

安装完成后（选择按需导入）：

![](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2020-03-14_20-56-26.jpg)

选择按需导入后，会在根目录生成`plugins`文件夹，文件夹下生成`element.js`文件，该文件内容：

```json
import Vue from 'vue'
import { Button } from 'element-ui'

Vue.use(Button)

```

如果需要使用到其他控件的时候，再过来按需导入比较合适。

由于安装`vue-cli-plugin-element`的同时，`main.js`中已经自动为我们加入了引用：
```js
import './plugins/element.js'
```
且`element.js`中默认导入了`Button`按钮，那么演示一下，现在启动项目，可以看到页面中有一个按钮叫做`el-button`，打开`App.vue`，找到`el-button`，增加一个`type`：

```
<el-button type="primary">el-button</el-button>
```

页面刷新后可以看到效果，说明`element-ui`导入成功。

如果需要导入其他控件，比如：`MessageBox`，只需要去`element.js`中：
```json
import Vue from 'vue'
import { Button, MessageBox } from 'element-ui'

Vue.use(Button)
Vue.use(MessageBox)

```

