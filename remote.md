## 操作远程仓库

### 添加远程仓库到本地

```
$ git clone ${remote_url}
```

\# 添加远程仓库时，所有的分支信息统统被下载到本地

### 查看关联了的远程仓库

```
$ git remote -v
origin	op@git.xjhng.top:OP/remote_repo.git (fetch)
origin	op@git.xjhng.top:OP/remote_repo.git (push)
```

### 为远程仓库地址指定简称

```
$ git remote add ${name} ${远程库地址}
此时看到：
first	op@git.xjhng.top:OP/remote_repo.git (fetch)
first	op@git.xjhng.top:OP/remote_repo.git (push)
origin	op@git.xjhng.top:OP/remote_repo.git (fetch)
origin	op@git.xjhng.top:OP/remote_repo.git (push)

origin是clone远程库时，默认指定的简称
first是刚用git remote add first ${远程库地址}来添加的
这样first和origin是等价的。一般就用默认的origin，可以使用git remote remove first。
```

### 从远程仓库拉取数据

```
$ git fetch ${简称} [${分支名}]
```

fetch只会将远程分支数据下载到你本地，并不会去合并分支或修改你当前的工作内容(即不会切换你所处的分支)。
fetch后会得到名为`${简称}/${分支}`的分支，随后`git merge ${简称}/${分支}`即可将当前分支与fetch得到的分支合并。

如果当前分支设置了"跟踪远程分支"，那么可以使用

```
$ git pull 
```

来自动拉取"被跟踪的远程分支"并合并到当前分支。
默认clone操作会自动设置本地master分支跟踪被克隆的仓库的master分支(也可能叫main分支)，执行git pull会尝试自动拉取远程库的master并合并到本地master

### 推送到远程仓库
准确说，你是推送指定的分支到远程仓库。注意：你必须有远端服务器的写入权限，更重要的是【要求没有人在你之前推送过】，否则别人推送了，你再直接推送，会被拒绝。
在这种情况下，你必须先拉取最新的数据，再合并，然后推送。

```
$ git push ${简称} ${本地分支}

例如：git push origin master  # 这样你就推送了本地的master分支来更新远程的master分支
```

【如果本地分支和远程分支名称不同】

```
$ git push origin master:main 
# 这将推送本地的master分支来更新到远端的main分支，如果远端没有main，远程仓库就新增main分支
```

### 查看远程仓库
```
$ git remote show ${简称}
```

