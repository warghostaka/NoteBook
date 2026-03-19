# git命令

记录一些常用的git命令，顺序不定

```bash
# 查看远程仓库名
git remote

# 查看所有远程仓库url
git remote -v

# 查看指定远程仓库的url
git remote get-url origin

# 撤销所有暂存文件
git reset

# 撤销指定文件的暂存
git reset -- <file>

# 查看文件的跟踪状态
git status

# 从git中移除已被跟踪的目录，使用-r参数递归删除目录
git rm -r --cached data
```

