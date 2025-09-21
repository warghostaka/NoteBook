# 创建git本地仓库以及连接到远程仓库

---

###### 2025年1月2日10:45:45

专业实训第一天在创建vue项目时，使用了git相关命令，故记录

### 创建仓库流程

首先在github上创建远程仓库，创建完成后再创建本地仓库并进行连接

实例：完成远程仓库的创建

![image-20250102105131627](C:\Users\WarghostAKA\AppData\Roaming\Typora\typora-user-images\image-20250102105131627.png)

随后根据这里的提示创建本地仓库

1. **创建 README 文件**：使用 `echo` 命令创建并写入 `README.md` 文件（非必须）

  ```bash
  echo "# test" >> README.md
  ```

2. **初始化 Git 仓库**：使用 `git init` 将当前目录初始化为 Git 仓库。

  ```bash
  git init
  ```

3. **暂存更改**：使用 `git add` 将 `README.md` 文件添加到暂存区（非必须）

  ```bash
  git add README.md
  ```

4. **提交更改**：使用 `git commit` 将暂存区的更改提交到本地仓库

  ```bash
  git commit -m "first commit"
  ```

5. **重命名分支**：使用 `git branch -M` 将默认分支重命名为 `main`

  > `main`是github默认使用的主分支名称，而git默认的主分支是`master`，因此修改分支名称

  ```bash
  git branch -M main
  ```

6. **关联远程仓库**：使用 `git remote add` 添加远程仓库并命名为 `origin`

  `origin`是一个常规的远程仓库别名，后续使用这个名称来指代某一个远程仓库

  ```bash
  git remote add origin https://github.com/WarghostAKA/test.git
  ```

7. **推送代码**：将代码推送到远程仓库，并将当前分支与远程分支关联

```bash
git push -u origin main
```

> 已经关联远程仓库的分支，再次提交时可以直接使用命令
>
> ```bash
> git push
> ```