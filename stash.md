## 保护现场

场景描述

​		你在dev分支上工作到一半，但是还没做完，不足以commit。因为一些事情你需要切换到别的分支(比如你要切换到bug分支，要紧急修复一个bug并发布)，如果你直接用checkout切换分支，git会阻止你，提醒你在当前分支还没有提交。因为如果不提交所有更改就切换分支，那么这些更改就会丢失(因为它们不曾托管到git过)。

​		这是很常见的操作，这时你就需要先保护现场，然后再切换分支。

#### 使用stash命令

**贮藏改动**

```
$ git stash
Saved working directory and index state WIP on dev: f31de78 更新 'log.md'
$ git status
On branch dev
nothing to commit, working tree clean
```

可以看到，你在当前分支的所有改动都被存起来了，它们被存储在称为”栈“的地方，现在工作区恢复成clean状态。现在，你可以像平常一样工作，正常切换到bug分支即可。

**查看**

这里我们将贮藏的改动信息称为“存档”，现在我们看看有哪些存档

```
$ git stash list
stash@{0}: WIP on dev: f31de78 更新 'log.md'
```

可见当前只有一份存档，名为"stash@{0}"，是在dev分支上做的存档。

**应用**

现在，你已经处理好了bug，你要切换回dev分支，并要恢复现场。

```
$ git stash apply stash@{0}  # 可不加存档名，默认取最近一次的
```

**删除**

该次存档没用了，想删掉

```
$ git stash drop stash@{0}
```

**一步完成应用+删除**

```
$ git stash pop # 应用栈中最近一次的“存档”
```

**说明**

1. apply时不要求工作区是否是clean的
2. apply时不要求分支与创建时一致
3. 如果工作区不是clean的，且apply时不能直接应用，则git会提示合并冲突，手动处理冲突
4. stash只包含已被git跟踪的文件，如果保护现场时需要包括未被跟踪的文件的状态，请用-u选项；使用-u选项后仍不含.gitignore中明确要忽略的文件，如果要包含，使用-a/--all