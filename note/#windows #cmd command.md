#### 查找端口号是否占用

```bash
netstat -ano|findstr "3306"

#返回结果示例：
  TCP    0.0.0.0:3306           0.0.0.0:0              LISTENING       4488
  TCP    127.0.0.1:3306         127.0.0.1:55911        ESTABLISHED     4488
  TCP    127.0.0.1:55911        127.0.0.1:3306         ESTABLISHED     14600
  TCP    [::]:3306              [::]:0                 LISTENING       4488
```

最后一列4488，就是`pid`



#### 根据pid查找对应进程

```bash
tasklist /fi "pid eq 4488"

#返回结果示例：
映像名称                       PID 会话名              会话#       内存使用
========================= ======== ================ =========== ============
mysqld.exe                    4488 Services                   0     22,316 K

#tasklist 具体使用方法请输入 tasklist /? 自行查阅
```

也可直接在`任务管理器`中查看。



### 将命令输出到文本

```bash
netstat -a -n -o >D:port.txt
```

