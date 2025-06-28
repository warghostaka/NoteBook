# 创建git本地仓库以及连接到远程仓库

---

###### 2025年1月2日10:45:45

专业实训第一天在创建vue项目时，使用了git相关命令，故记录

---

### 创建仓库流程

首先在github上创建远程仓库，创建完成后再创建本地仓库并进行连接

实例：完成远程仓库的创建

![image-20250102105131627](C:\Users\WarghostAKA\AppData\Roaming\Typora\typora-user-images\image-20250102105131627.png)

随后根据这里的提示创建本地仓库

1. **创建 README 文件**：使用 `echo` 命令创建并写入 `README.md` 文件（非必须）

	```
	echo "# test" >> README.md
	```

2. **初始化 Git 仓库**：使用 `git init` 将当前目录初始化为 Git 仓库。

	```
	git init
	```

3. **暂存更改**：使用 `git add` 将 `README.md` 文件添加到暂存区（非必须）

	```
	git add README.md
	```

4. **提交更改**：使用 `git commit` 将暂存区的更改提交到本地仓库

	```
	git commit -m "first commit"
	```

5. **重命名分支**：使用 `git branch -M` 将默认分支重命名为 `main`。

	```
	git branch -M main
	```

6. **关联远程仓库**：使用 `git remote add` 添加远程仓库并命名为 `origin`。

	```
	git remote add origin https://github.com/WarghostAKA/test.git
	```

7. **推送代码**：使用 `git push -u` 将本地 `main` 分支的代码推送到远程仓库，并设置上游分支。

------

### 出现的问题

由于操作不当导致提交时出现错误：

`\> git pull --tags origin main From https://github.com/WarghostAKA/Training * branch            main       -> FETCH_HEAD 
fatal: refusing to merge unrelated histories`

#### **原因**

1. **两个仓库的历史不相关**：
	* 你尝试合并的两个分支（例如 `main` 和 `origin/main`）没有共同的提交历史。
	* 这可能是因为你初始化了一个新的本地仓库，而远程仓库已经存在一些历史记录。
2. **克隆或拉取时未关联历史**：
	* 如果你克隆了一个仓库，但本地仓库已经存在一些提交，Git 会认为这两个仓库的历史不相关。

#### 解决办法

如果本地仓库的历史不重要，可以重置本地仓库，使其与远程仓库完全一致：

1. 备份本地更改（如果有需要）：

	```
	git stash
	```

2. 重置本地 `main` 分支：

	```
	git fetch origin
	git reset --hard origin/main
	```

3. 恢复本地更改（如果有备份）：

	```
	git stash pop
	```



或者干脆直接删除本地仓库（.git文件），然后重新进行初始化

------