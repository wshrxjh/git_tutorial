1. 所有文件都只有已被追踪(tracked)和未被追踪(untracked)的状态

2. 再细分一点，所有文件都只有以下几种状态
$ git status -s
A  CONTRIBUTING.md
# 每个文件名前有个状态标识符
?? 表示未追踪的文件
A  表示新添加到暂存区的文件
M  表示修改过的文件

