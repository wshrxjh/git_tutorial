$ git log 
可以查看所有的提交历史，最新的在最上面，格式如下：
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gmail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test code


当历史过多时，你可以指定查看多少条，如查看五条提交历史
$ git log -5

一个很常见的想法是，你想要查看某个时间段的提交信息
时间格式很灵活，--since和--until可以仅用其一，按你想要的来试试就对了
$ git log --since="2020-10-01 00:00:00" --until="2020-10-03 11:00:00"

最好用的可能还是通过关键字搜索提交信息
$ git log --grep "监控"

你想看稍微详细点的记录，比如哪些文件做了修改，那么可以
$ git log --stat


你想看更详细的记录，详细到文件内容的变化，那么可以
$ git log -p  # 文本文件的差异内容以补丁格式输出




关于输出内容的格式，--pretty提供了多种选择
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 (HEAD -> master, origin/master, origin/HEAD) changed the verison number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

通过--pretty的子选项format，你甚至可以自定义输出内容格式
$ git log --pretty=format:"%h - %an, %ar : %s"
