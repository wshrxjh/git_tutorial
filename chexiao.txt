在开发环节的任何时候，你都可能会有想要"撤销"的想法，这很常见。
但要注意，即使是用git，某些场景下，一旦撤销，也是没有后悔药可吃的。

场景一：我都提交完了，才发现有几个文件漏了，即忘记add和commit它们了，又或者你的提交信息写错了
$ git commit --amend
这会创建一个新的提交，会覆盖之前的提交。从原理上来说，这并不是用新的提交替换旧的提交，然后隐藏旧的提交。
使用--amend，旧的提交就像从未出现过一样，故这是"不可逆"的操作。
如果文件没有问题，只是要修改上次提交时编辑的提交信息，那么暂存区保持空，使用git commit --amend就对了，
使用这个选项后git不会因为暂存区为空而报错，而是直接让你编辑一个文本文件。


场景二：添加了三个文件到暂存区，其中有个不需要，我想要将它从暂存区撤销掉怎么办？
前面提到过git rm --cached xx，这是同时删除暂存区和git仓库的，并不适合此场景。
用git status就能看到git的贴心提醒了：
$ git reset HEAD xx
看到这个命令，很可能无法从字面意思去理解，这里先不谈，放到后面。


场景三：我重写了某个文件，突然发现不该这么改，我想把它恢复成编辑前的样子(准确说是刚clone时的样子，或最近一次commit
后的样子)，但不影响到别的文件，怎么办？
git status同样贴心地提醒你了
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   xx
$ git checkout -- xx
注意，前提是改动后的文件未添加到暂存区，否则不生效，如果你添加到暂存区了，可以先参考场景二来unstaged该文件，再执行
git checkout -- xx;
这是一个不可后悔的操作，git会用最近提交的版本来覆盖你工作区的该文件。
