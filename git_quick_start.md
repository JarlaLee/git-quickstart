## 学习目标

- 了解 Git 基本概念
- 能够概述 Git 工作流程
- 能够使用 Git 常用命令
- 熟悉 Git 代码托管服务
- 能够使用 IDEA 操作 Git



## Git 的作用

- 代码工程备份
- 代码还原 -- 代码修改的面目全非
- 协同开发同一个项目
- 追溯问题代码的编写人和编写时间
  - 比如查找谁修改bug很慢



## 版本控制器的方式

**集中式版本管理器**

这种类型的管理工具，版本库是集中存放在中央服务器的，team里每个人work时从中央服务器下载代码，是必须联网才能工作，这里联网必须是局域网或者互联网。个人修改后然后提交到中央版本库。



**分布式版本控制工具**

分布式版本控制系统没有中央服务器，每个人的电脑上都是一个完整的版本库。多人协作只需要各自的修改推送给对方，就能互看到对方的修改了。



## Git 工作流程图

![image-20250507224830528](E:\Data\Notes\Git\assets\image-20250507224830528.png)

- clone: 从远程仓库克隆代码到本地仓库
- checkout: 从本地仓库中检索处一个仓库分支，然后进行修订
- add: 将代码提交到暂存区
- commit: 提交到本地仓库 local repository。本地仓库中保存修改的各个历史版本
- fetch: 从远程仓库 Remote Repository 拉取到本地仓库，不进行任何的合并merge 动作。
- pull 从remote 拉取到 local ，自动进行 merge，然后到工作区，相当于 fetch + merge
- push：修改完成后，将代码推送到远程仓库。



## 初始化Local Repository

在根目录中，打开git bash 窗口, 执行:

- git init 命令， 初始化成功后, 可以在文件夹下面看到隐藏的.git目录



## 基础操作指令

git 工作目录下对于文件的修改(增加，删除，更新文件内容) 会存在几个状态，这些修改的状态会随着我们执行git的命令而发生变化。

三种状态:

- untracked 在一个git项目中，新创建一个文件，而没有使用git add 添加该文件到 index 区。
- unstaged 在一个git项目中，修改一个文件中的内容，而没有使用git add 添加该文件到 index 区。
- staged 改动被 git add 之后，处于 index区域的状态。

![image-20250507235726426](E:\Data\Notes\Git\assets\image-20250507235726426.png)



### git add

将改动文件从 工作区 到 暂存区。

git add 文件名 --> 将某个具体file 添加到暂存区

常用的指令 git add . 将workspace中所有的改变添加到 index 区域



### git commit

git commit 将暂存区的改动文件，提交到本地仓库 local 中。一般需要添加commit 的消息。常用

git commit -m "本次commit 的功能内容描述"



### git status

git status 可以用来查看workspace中的文件的状态。



### git log

可以用于查看 git commit 的历史。会显示git commit 的消息语

git log [option]

options:

- --all
- --pretty=oneline 将提交信息显示为一行
- --abbrev-commit 使得输出的commitedid 更短



### 简单的回退

```git
git log // 找到每次commit 的标识


git reset --hard a975438
```





### 如何查看已经删除的记录

git reflog 这个命令可以看到已经删除的提交记录。



### 添加文件至忽略列表

有一些文件因为内容隐私等无需纳入git的管理，也不希望他们出现在文件跟踪列表。比如日志文件，比如编译过程中创建的临时文件。需要创建以一个.gitignore的文件，列出需要忽略的文件模式。

```java
# no .a files
.a

## 忽略doc文件夹下的notes.txt文件，但是不忽略 doc/server/arch.txt文件
doc/#.txt
    
# 忽略 所有的 .pdf 文件，在 doc/ 文件夹下
doc/**/*.pdf
    
    
```





### git 分支

使用分支，你就可以把你的工作从开发主线上分离开，进行的重大的Bug修改，开发新的功能，避免影响开发主线。



### git branch

查看本地分支



### git branch 分支名

创建一个新的本地分支



### git checkout 分支名

切换分支。



### 直接切换到一个不存在的分支 (创建 + 切换)

git checkout -b 分支名



### 合并分支

一个分支的commit 可以合并到另一个分支

在当前分支上，使用git merge 另外一个分支的名字， 将另外的分支合并到当前的分支上。





### 删除分支

git branch -D b1 不做任何检查，强制删除



### 解决冲突

当两个分支上对文件的修改可能存在冲突，例如同时修改了同一个文件的同一行，这时就需要手动解决冲突。解决冲突的步骤：

- 处理文件中冲突的地方
- 将解决完冲突的文件加入暂存区
- 提交到仓库 commit

```git
<<<<<<< HEAD
update count=3
=======
update count=2
>>>>>>> dev

HEAD 到 ==== 之间的代码表示当前branch的代码。
===== 到 dev 之间的代码就是 merge 到这个分支上的代码。也就是其他分支的代码


# 解决冲突之后:
git add . 添加到暂存区

git commit 提交到本地仓库
```



## 分支使用的流程

master 分支： 生产分支，主分支。中小规模的项目作为线上运行的应用对应的分支。

如果代码需要上线，就需要时master上的代码

develop 分支

是从master 创建的分支，这里就是开发部门的开发分支。开发阶段完成之后，需要合并到master分支，准备上线。

feature/xxx分支

从develop创建的分支，同期并行开发，不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支。

hotfix/xxxx分支

从master派生的分支，一般作为线上bug修复使用，修复完成后合并到master, test, develop分支。

test分支

用于代码测试



## 仓库托管

- git
- gitee
- 企业中用gitlab （git，gitee 收费，可能不安全）



在云端创建一个remote 仓库。

配置SSH公钥 - 生成SSH 公钥

Gitee 账户获取公钥

验证是否配置成功

ssh -T git@gitee.com



### 远程仓库添加

找到仓库地址。

如何把本地仓库推送到远程仓库。

- 绑定本地和remote仓库
  - git remote add origin 远程仓库地址
    - 这里origin 就是远程仓管库在本地的名字。
  - git remote 检查远程仓库
- 推送到远程仓库
  - git push origin master 推送本地master分支，推送到remote 仓库
    - 完整的形式 git push 远端名字 本地分支名: 远端分支名
    - git push origin master: master 如果远端和本地是一样的可以省略，只写一个
- --set-upstream 推送到远端的同时建立起和远端分支的关联关系
  - git push --set-upstream origin master
- 如果当前分支已经和远端分支关联，可以省略分支名和远端名。master和origin
  - git push



如果你想使用git push这样的命令，你就必须使用up-stream进行绑定。

git push --set-upstream origin master:master

将本地的master分支与云端origin master分支绑定好

之后，你就可以直接git push了



[18_远程仓库复习_添加_查看_推送_关联关系_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1MU4y1Y7h5?spm_id_from=333.788.videopod.episodes&vd_source=2f39b7a71778e6e5f3139416ebc6a414&p=18)

### 远程仓库查看





### 远程仓库推送













