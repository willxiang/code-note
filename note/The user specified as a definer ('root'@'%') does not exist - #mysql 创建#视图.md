找到mysql安装目录，使用命令行登录：
```
mysql -h localhost -u root -p
```
然后输入密码后进入mysql，给root账号添加权限：
```
GRANT ALL PRIVILEGES ON *.* TO 'USERNAME'@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;

FLUSH PRIVILEGES; 
```
需要自行修改命令中的USERNAME和PASSWORD
然后重试。