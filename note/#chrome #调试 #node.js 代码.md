### Chrome 调试node.js 代码

例如调试node-red：

```
node --inspect /C/Users/willx/AppData/Roaming/npm/node_modules/node-red/red.js
```

运行后会看到类似输出：

```
Debugger listening on ws://127.0.0.1:9229/decc2145-88d7-45f2-b6d2-0e2c71228ef5
For help, see: https://nodejs.org/en/docs/inspector
```



Chrome浏览器输入：

```
chrome://inspect
```

稍等片刻后会看到：

![https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2019-11-05_16-40-00.jpg](https://raw.githubusercontent.com/willxiang/code-note/master/img/Snipaste_2019-11-05_16-40-00.jpg)

然后就可以开始调试了