## reset的两种用途

reset：Reset current HEAD to the specified state

所以，reset命令，就是用来改变HEAD这个指针的指向的。

官方图文讲解reset：

[Git-重置揭秘](https://www.git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)

#### 第一种用法：

要了解reset，上面的链接说明再清楚不过。这里简单说明：

> 假设我们刚在master分支commit一次，现在，git有99次提交，此时即有99份快照。HEAD指针是指向master，准确说说是指向第99份快照，
>
> **当前**
>
> \#   假设此时最近一次commit的hash值是AAAAA
>
> HEAD ->  v99
> INDEX -> v99
> workspace -> v99
>
> **当我们在工作区做了任何改动后**
>
> HEAD ->  v99
> INDEX -> v99
> workspace -> v100
>
> **此时工作区是脏的，当我们git add test.py后**
>
> HEAD ->  v99
> INDEX -> v100
> workspace -> v100
>
> **再当我们git commit后**
>
> \# 现在最近一次commit的hash值是BBBBB
>
> HEAD ->  v100
> INDEX -> v100
> workspace -> v100

reset是用来移动HEAD这个指针的指向的，更准确的说“移动指针使HEAD指向当前分支的某次提交”。现在HEAD指向最近的一次快照，即HEAD -> BBBBB

#### 移动HEAD

**--soft**

```
git reset --soft AAAAA
```

HEAD ->  v99
INDEX -> v100
workspace -> v100

**--mixed（默认）**

```
git reset --mixed AAAAA
```

HEAD ->  v99
INDEX -> v99
workspace -> v100

**--hard**

```
git reset --hard AAAAA
```

HEAD ->  v99
INDEX -> v99
workspace -> v99

#### 小结

只有--hard才会覆盖工作区的文件。

使用--soft或--mixed都不危险，你可以reset回到 BBBBB 对应的快照。

所以--hard才是最危险的，当你使用此方式回到v99后，工作区会被强制覆盖，当然git还是有v100对应的快照，你可以回到v100的版本。

> **如果你在那之后又工作了一段时间，但没提交，add与否不重要**
>
> HEAD ->  v100
> INDEX -> v100 或 v101
> workspace -> v101

那么 git reset --hard BBBBB 后，v101所作的改动就真正全部丢失了。

------

#### 第二种用法

在第一种用法中，reset做的是恢复到某一快照版本。

`reset` 命令按如下逻辑执行，在你指定以下选项时停止：

1. 移动 HEAD 分支的指向 *（若指定了 `--soft`，则到此停止）*
2. 使暂存区同步 HEAD *（若指定了 `--mixed`，则到此停止）*
3. 使工作区同步暂存区*（若指定了 `--hard`，才执行此步骤）*



在第二种用法中，reset可以指定将指定的文件和目录恢复到某一版本，未被指定的文件或目录不作变动。

需要注意的是，如果你指定了文件或目录，由于HEAD 只是一个指针，你无法让它同时指向两个提交中各自的一部分，上述三步走的第1步会跳过。 不过索引和工作目录 *可以部分更新*，所以重置会继续进行第 2、3 步。即第二种用法只影响暂存区和工作区。

**例子：**将文件或目录从暂存区移除

```
$ git status
On branch dev
Changes to be committed:
	(use "git reset HEAD <file>..." to unstage)

你常常会看到git提示你使用reset将文件从暂存区移除。其实就是将暂存区中指定文件/目录同步成HEAD指向的版本
$ git reset HEAD test.py 
    等效于 git reset --mixed HEAD test.py 
    	  [如果git版本比较新，会提示：warning: --mixed with paths is deprecated; 
    	  use 'git reset -- <paths>' instead.]
    等效于 git reset test.py

```

**例子：**将某文件或目录恢复到某一历史版本

在上一个例子中，我们用reset HEAD表示将暂存区指定的文件恢复到HEAD版本，而HEAD就是个指针，所以本质上你可以对某文件在暂存区与工作区恢复到任意版本，只需要直到对应版本的hash值即可。

```
git reset AAAAA main.py
git reset BBBBB --hard main.py
```

