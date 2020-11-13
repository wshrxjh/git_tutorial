LALALA
## 修改文件名

git并不显式追踪文件移动/改名操作，你更改文件名的操作，git仓库存储的元数据无法体现出这一行为。

 

你必须告诉git

```
git mv file_old_name file_new_name
```



使用$ git status可看到提示 renamed: file_old_name -> file_new_name



而实际上，git就是做了如下三个操作

```
mv file_old_name file_new_name
git rm file_old_name
git add file_new_name
```



常见错误示范

```
mv example.txt example.md
git add example.md
git commit -m "说明文件改用markdown"
git push origin master

你能push成功，此时你本机工作区显示只有example.md。
但实际上本机仓库和远端库中，同时存在example.txt和example.md。
如果你用git status，就能看到git提示你删除example.txt。
```
