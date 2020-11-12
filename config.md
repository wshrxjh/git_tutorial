## Git基本环境配置

三种等级(优先级从高到低)

1. local：配置会写入${path_to_project}/.git/config 文件
2. global：配置会写入当前用户的 ~/.gitconfig
3. system：配置会写入/etc/gitconfig



一般就按最细的来配，当新建或新克隆了一个项目后

```
git config user.name "Bitter"
git config user.email "wshrxjh@sina.com"

git config --local --list
```