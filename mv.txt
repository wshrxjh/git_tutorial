git并不显式追踪文件移动/改名操作，你更改文件名的操作，git仓库存储的元数据无法体现出这一行为。
$ git mv file_old_name file_new_name

使用$ git status可看到提示
 renamed:    file_old_name -> file_new_name


而实际上，git就是做了如下三个操作
$ mv file_old_name file_new_name
$ git rm file_old_name
$ git add file_new_name
