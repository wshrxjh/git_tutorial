## 基础协作流程

假设当前github/gitea有一个仓库wshrxjh/tutorial，我的账号名是Bitter

1. 浏览器访问该代码仓库，点击项目首页右上角的Fork，由wshrxjh/tutorial“派生”得到Bitter/tutorial

2. 将fork出的副本clone到本机，即你做开发的机器上，这样你的本地机器上就有了tutorial项目所有文件，此时你处于master分支；同时当前repo下自动添加了名为“origin”的remote url

3. 创建一个dev分支并切换到该分支：

   ```
   git checkout -b dev
   ```

4. 在dev分支上开发，完成修改检查和测试代码的工作

5. 提交你的更改

6. 将dev分支push到副本库；

   ```
   git push origin dev
   ```

7. 浏览器进入Bitter/tutorial，可以看到服务器上多了个dev分支

8. 提交pull request(合并请求)，请求 wshrxjh:master <-- Bitter:dev

9. 源仓库管理员wshrxjh打开浏览器进入wshrxjh/tutorial，查看Pull Request，同意后即可合并分支。

这种模式下，Bitter/tutorila的dev分支本就是基于wshrxjh/tutorial的master的，因此最终合并时，相当于wshrxjh/tutorial的master分支使用fast foward模式无冲突地顺利更新到下一版本。



## 当源库有改动后

由于是多人协作，在合并你的请求时，可能源库已经更新过了(例如合并过别人的请求)，这时就无法一键合并，因为此时已存在冲突，无法fast-forward。

假定按照上面的流程，你现在位于dev分支上，已经在dev分支开发好了新功能并已提交

1. 添加一个远端信息（源库），通常命名为upstream

   ```
   git remote add upstream https://github.com/wshrxjh/tutorial.git
   ```

2. 拉取源库最新版本

   ```
   git fetch upstream
   ```

3. 将源库版本合并到dev分支

   ```
   git merge upstream/master
   ```

   这一步，如果存在冲突，git会提示你

4. 修复冲突，然后提交，此时提交，git判断冲突解决后会自动完成上一步的merge操作，无须再手动git merge了。此时合并完成，对于源库来说，当前版本其实就是基于源库版本的只相差一次提交的“新版本”，因此后续可以fast forward合并到源库

5. 将dev分支push到副本库；

   ```
   git push origin dev
   ```

6. 后续步骤同上：提交合并请求，源库管理员审批



## 似乎忽略了派生库的master

在上面介绍的工作流种，似乎派生库的master完全不用，而是仅用“多此一举”创建的dev分支。

派生库的master是为了保持与源库同步，当源库有新的提交后，GitHub会提示你 “This branch is 2 commits behind wshrxjh:master.”，但是GitHub并不会帮你更新以保持同步。

手动更新

```
# 切到master分支
git checkout master 

# 拉取原生库的master分支合并到当前分支(fetch仅拉取，pull会拉取并合并)
git pull upstream master  

# 推送到派生库
git push origin master
```

这样你的master库就与源库保持一致了。



## 综上所述

1. fork源库到自己的账号下，得到派生库
2. clone自己的派生库到本机，clone后你位于master分支
3. 创建并切换到新分支：如dev分支或bug分支
4. 添加源库地址 git remote add upstream https://xxxx
5. 现在你可以开发了
6. 开发完提交
7. 把dev分支内容推送到源库并创建合并请求，审批通过后成功合并
8. 等待下次又要更新或调整代码
9. 在本机，切到master下，将派生库的master与源库的master同步一下
10. 删除本机的dev分支
11. 回到第2步

