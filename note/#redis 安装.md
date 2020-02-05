安装redis服务：
```
redis-server --service-install redis.windows.conf
```

启动redis服务：
```
redis-server --service-start
```

默认端口为：6379，如需更改，请在 redis.windows.conf 查找。


常用的服务命令

卸载服务：redis-server --service-uninstall

开启服务：redis-server --service-start

停止服务：redis-server --service-stop