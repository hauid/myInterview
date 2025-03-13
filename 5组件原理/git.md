**集中式版本控制系统SVN同样有**

分布式版本控制系统

1.分布式版本控制系统**根本没有“中央服务器”**，每个人的电脑上都是一个完整的版本库(github只是逻辑意义上的中央服务器)

2.安全性要高很多



工作区,暂存区,版本区是同一块区域,每次**push只是把修改的东西提交**

(Linux系统思想)



<img src="C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250228160738576.png" alt="image-20250228160738576" style="zoom: 66%;" />

**每个库都有三个区**
三个区的基本概念
**工作区 -> 暂存区(快照) -> 版本库**

对本地仓库常用命令:
git status 								查看状态
git add										加入暂存区
git reset									取消暂存或者切换到指定版本
git commit  -m[注释]				暂存区提交到版本库
git log										查看日志
git reset hard [版本号(通过log来查看)]			切换到指定版本

对远程仓库常用命令:
git remote													查看远程仓库
git remote add											添加远程仓库
**git clone 													从远程仓库克隆**
git pull	[short-name]	[分支]				从远程仓库拉取
git push	[远程仓库代号]	[分支]		推送到远程仓库

对分支常用命令:
**git branch										查看分支**
**git branch	[name]						创建分支**
**git checkout 	[name]						切换分支**
git push	[远程仓库代号]	[分支]			推送到远程仓库分支
**git merge	[name]						合并分支**
**git branch	-d	[name]					删除分支**



要查看远程仓库中的所有分支，可以使用以下命令：

**git branch -r**

win+s 输入字符映射表 没解决 暂时就复制

- `├`：Alt + 195
- `─`：Alt + 196
- `└`：Alt + 192



### 1. **Git Fetch**

`git fetch` 命令用于从远程仓库**下载最新的数据到你的本地仓库**，但它**不会自动合并或修改你的当前工作**。`fetch`只是将远程的数据拉取到本地的远程跟踪分支中（通常是 `origin/main`，`origin/master` 等）。这允许你在合并更改到你的工作分支之前，先审查这些更改。

- **使用场景**：当你想要看到远程仓库中有哪些更新，但还不准备将这些更改合并到你的工作中时，使用`fetch`是很有用的。

### 2. **Git Pull**

`git pull` 命令基本上是 **`git fetch` 后跟 `git merge`** 的快捷方式。当你执行 `pull` 时，Git会从远程仓库拉取最新的数据并自动尝试合并到你当前的分支。这个命令简化了工作流程，但有时也可能导致不期望的合并冲突。

- **使用场景**：当你确信想要将远程分支的更改合并到你的当前分支，并且准备处理可能出现的合并冲突时，使用`pull`是合适的。

### 总结

- **`git fetch`**：**安全地获取远程数据，让你有机会手动审查和合并。**
- **`git pull`**：**更方便地获取并自动合并远程数据，但有时可能需要处理合并冲突。**

如果你想从名为 `origin` 的远程仓库获取 `feature-branch` 分支，你应该执行：

```
git fetch origin feature-branch
```





###  git pull 指定远程仓库和分支

使用 `git pull` 命令时，你可以指定远程仓库的名称和远程分支的名称来从中拉取更新。格式如下：

```
git pull <remote-name> <branch-name>
```

- `<remote-name>` 是你想要拉取的远程仓库的名称。
- `<branch-name>` 是远程仓库中你想要拉取的分支名称。

例如，如果远程仓库名称是 `origin` 并且你想要拉取 `main` 分支的更新，你应该执行：

```
git pull origin main
```



### **通配符使用错误**：

 在某些命令行环境中，星号（`*`）可能需要引用以避免被 shell 解释为当前目录下的所有文件。你可以尝试使用引号来确保星号被正确处理：

```
git branch -r --list 'origin/*'
```









### 个人日常常用命令

git  pull = git fetch+git merge

git pull origin main							从origin仓库拉取main分支



git branch -r --list 'mygithub/*' 		查看分支



