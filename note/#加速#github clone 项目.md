开启ss后，只针对github的设置：
```
git config --global http.https://github.com.proxy https://127.0.0.1:1080
git config --global https.https://github.com.proxy https://127.0.0.1:1080
```
然后clone时使用http(s)的地址去clone。

