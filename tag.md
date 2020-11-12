标签，是作用于仓库中的某一次具体提交的，打上标签往往是做个记号，表示重要，例如给某次提交打上标签"v1.0"

列出所有标签
$ git tag

列出过滤后的标签
$ git tag -l "v2.*"

创建标签
	git支持"轻量标签"和"附注标签". "轻量标签"就是对某次特定的提交的引用. 而"附注标签"是存储再git库中的一个对象，它包含"打标签者的人名、邮箱、时间、
	一个打标签者自定义的标签信息(使用-m追加的描述)",此标签还可通过GPG签名和安全认证。轻量标签本质上就是将commit的哈希值存储到一个文件中。
    对当前分支的最近一次提交打标签
	附注标签：$ git tag -a v1.0 -m "App version 1.0"
	轻量标签：$ git tag v1.0
    对别的提交打标签
	$ git tag -a v1.0 -m "App version 1.0" ${hash值}
	$ git tag v1.0 ${hash值}

删除标签
	$ git tag -d v1.0

查看标签的详细信息
	$ git show v1.0


标签与远程库
	注意：git push不会把标签信息推送到远程库，你必须显式推送它们;相应的，你删除标签，也不会影响远程库
	$ git push origin ${tagname}
	
	如果要推送很多标签，必须添加--tags
	$ git push origin --tags

	要删除远程库的标签，必须
	$ git push origin --delete ${tagname}


