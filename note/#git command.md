### 回退本地版本

```
git log
git reflog
```

使用以上命令查看需要回退版本的commit id.

假设当前需要回退的id为：**ddf756abbbb**

```
git reset --hard ddf756abbbb
```

此时本地版本将回退到这个id的版本。

如果远程仓库也被影响，则此时应该强制push到远端，将远端也回退：

```
git push -f
```



### 推送本地已存在仓库到云端

```bash
git remote add origin git@github.com:willxiang/willxiang.git
git push -u origin master
```



---

### 切换远端仓库地址

查看当前远端地址：

```bash
git remote -v
# origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
# origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

修改当前远端地址：

```bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY_2.git
git remote set-url origin git@github.com:USERNAME/REPOSITORY_2.git
```

---

### 本地分支推送到远程（远程没有此分支就会自行创建）:

```bash
git push -u origin dev1:dev2
```

推送本地`dev1`分支到远程服务器，并创建分支，分支名为`dev2`，同时关联本地dev1与远程的dev2。
`-u` 是`--set-upstream`的缩写。

---

###  手动关联本地分支与远程分支：

```bash
git branch --set-upstream-to=origin/develop dev
```
上述命令的动作就是，把本地叫做`dev`的分支与远程叫做`develop`的分支做关联。

---

查看本地与远程分支的关联情况：
```bash
git branch -vv
```

---
### 切换到指定tag（与切换分支类似）

先查看tag
```bash
git tag
```
选择好想要切换的tag（记住名字或者复制一下）即可


```bash
git checkout -b new_dev_tag_branch_name target_tagname
```

---

git默认的push.default是simple模式，要求两边分支同名，而upstream模式则不做这个要求，所以可以修改push的模式：
```
git config --global push.default upstream
```
这样的话，本地分支可以不与远程分支名称相同，然后关联分支关系后，可以直接git push，而不需要每次都指定分支名称。

---

#### 本地先打tag（给指定commit打tag）
```
git tag -a tagName commit_id -m "my tag"
```
`commit_id`: commit id
`tagName`: tag的名称  
`"my tag"`: tag的说明


#### 推送tag到远端仓库
```
git push origin tagName
```


#### 删除远端tag
```
git push origin :refs/tags/tagName
```



### 切换到远程的分支：

```bash
git checkout -b develop remotes/origin/develop
```
不知道切换到什么名字的分支，可先用`git branch -a`查看。

---

### 删除本地未提交的新文件
```bash
git clean -nfd
git clean -fd
```
`n`表示不会真的删除，会先模拟操作，告诉你会删除什么文件，去掉`n`后是真正执行，保险起见请先执行带`n`参数的命令查看结果

---

### 删除远程分支

```bash
git push origin --delete branchName
```
假如我们现在查看远程分支名称：
```bash
git branch -a
origin/HEAD -> origin/master
origin/develop
origin/master
```

如果我们要删除develop分支，则：

```bash
git push origin --delete develop
```

---
### 配置git

配置用户名跟邮箱地址：
```bash
git config --global user.name "willxiang"
git config --global user.email  "willxiangs@gmail.com"
```

查看配置的变量列表
```bash
git config --list
```

### 检查是否存在ssh key

```bash
cd ~/.ssh
```
进到此目录，如果该目录没有任何文件，则不存在ssh key

生成ssh key
```bash
ssh-keygen -t rsa -C "willxiangs@gmail.com"
```
一路回车，然后会生成id_rsa 和 id_rsa.pub文件

将公钥配置到相关仓库服务器的配置中即可
```bash
cat id_rsa.pub
```

---
### git config 配置命令

config配置有3个层级：

- system（系统级别）
- global（用户级别）
- local（仓库级别）

覆盖优先级为local > global > system。优先读取local，其次是global，最后是system。

读取system级别的配置：

```bash
git config --system --list
```

读取global级别的配置：

```bash
git config --global --list
```

读取local级别的配置：

```bash
git config --local --list
```

如果想修改配置的话，加上不同的参数就可以在不同的级别上配置了。

比如配置global级别的信息：

```bash
git config --global user.name "yourusername"
git config --global user.email "youremail@email.com"
```



---
### 删除untracked files:
```bash
git clean -nfd
```
`n`参数表示执行该命令时是测试模拟执行，不会真的执行，因为是删除文件操作，所以建议先执行带`n`的命令，查看好会删除哪些文件，确认后再去掉`n`执行。

---

### 配置代理

`shadowsocks`:

```bash
git config --global http.https://github.com.proxy https://127.0.0.1:1080
git config --global https.https://github.com.proxy https://127.0.0.1:1080
```

`clash`：

```bash
git config --global http.https://github.com.proxy socks5://127.0.0.1:7891
git config --global https.https://github.com.proxy socks5://127.0.0.1:7891
```

