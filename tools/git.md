**集中式版本控制系统SVN同样有**

分布式版本控制系统

1.分布式版本控制系统**根本没有“中央服务器”**，每个人的电脑上都是一个完整的版本库(github只是逻辑意义上的中央服务器)

2.安全性要高很多

工作区,暂存区,版本区是同一块区域,每次push只是把修改的东西提交

(linux系统思想)



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
git clone 													从远程仓库克隆
git pull	[short-name]	[分支]				从远程仓库拉取
git push	[远程仓库代号]	[分支]		推送到远程仓库

对分支常用命令:
git branch										查看分支
git branch	[name]						创建分支
git checkout 	[name]						切换分支
git push	[远程仓库代号]	[分支]			推送到远程仓库分支
git merge	[name]						合并分支
git branch	-d	[name]					删除分支



win+s 输入字符映射表 没解决 暂时就复制

- `├`：Alt + 195
- `─`：Alt + 196
- `└`：Alt + 192