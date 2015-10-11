title: "Git笔记"
date: 2015-03-21 08:21:32
category: [开发工具]
tags: [Git]
---
##前言
实验室的项目需要用到一个版本管理系统进行管理，鉴于Git如此风靡，所以正好拿来用，顺便学习一下。一下是Git学习的笔记，大部分内容来自[廖雪峰的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)。

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。所以一切操作几乎都是在一瞬间完成的。

##集中式VS分布式

集中式版本控制系统，版本库是集中存放在中央服务器的。所有的修改都要经过中央服务器。
分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。
**Git的特点就是，没有中央服务器，大家都是中央服务器**

##创建版本库
初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 第一步，使用命令`git add <file>`，可反复多次使用，添加多个文件；

2. 第二步，使用命令`git commit`，完成。

##查看状态
要随时掌握工作区的状态，使用`git status`命令。
 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。
 
##版本回退
![](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqd05t0do1j206j04imx4.jpg)
HEAD指向的版本就是当前版本(HEAD指向当前的分支)

 因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
 
返回过去，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
##工作区VS版本库VS暂存区
- 工作区（Working Directory）：就是你在电脑里能看到的目录
- 版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
- 暂存区(stage) : 版本库里存了很多东西，其中最重要的就是称为**stage**（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
![enter image description here](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqd05ta69vj20cq06imxh.jpg)

`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage）
执行`git commit`就可以一次性把暂存区的所有修改提交到分支。
**Git跟踪的是修改，每次修改，如果不add到暂存区，那就不会加入到commit中。**

##撤销修改
- 还没有`git add`到暂存区的代码
`git checkout` 从暂存区或者版本库签出（取消工作区代码）,例：`git checkout -- readme.txt`
1. 暂存区有代码，签出暂存区代码
2. 暂存区没代码，签出版本库代码

- 已经添加到暂存区的代码,`git reset HEAD readme.txt`
再用`checkout`命令

- 已经commit的代码
在推送到远程版本库之前切换版本,`git reset --hard` 版本号

- 删除文件(删除也是一个修改操作)
`git rm`用于删除一个文件（类似于git add将更改放到暂存区）
`git commit` 提交

##github远程仓库
提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库
###生成ssh密钥

```
ssh-keygen -t rsa -C "youremail@example.com" 
```

生成ssh 公钥和私钥
###关联远程库

```
git remote add origin git@server-name:path/repo-name.git
```
###推送
使用命令`git push -u origin master`第一次推送master分支的所有内容，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；
###克隆

```
git clone git@github.com:michaelliao/gitskills.git
```
##分支管理
Git用master（主分支）指向最新的提交，再用HEAD指向当前分支
查看分支：`git branch`
创建分支：`git branch <name>`
切换分支：`git checkout <name>`
创建+切换分支：`git checkout -b <name>`
合并某分支到当前分支：`git merge <name>`
删除分支：`git branch -d <name>`

###解决冲突
`Rudy@RUDY ~/learngit (master|MERGING)`
相当于另外创建了一个分支，保存冲突，提交时以这个分支为准

###分支策略
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以
![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqd05tptzoj20du03h3yy.jpg)

###修复bug，保存现场状态
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。
> git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
###新的feature分支
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

##多人协作工作
实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是`origin`。

试图推送：可以试图用`git push origin branch-name`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果git pull提示`no tracking information`
     说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

##标签管理
**发布一个版本时**，我们通常先在版本库中打一个标签。标签也是**版本库的一个快照**。

Git的标签虽然是版本库的快照，但其实**它就是指向某个commit的指针**（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

查看所有标签`git tag`

新建标签`git tag <name>`，默认为HEAD，也可以指定一个commit id；

指定标签信息,` git tag -a <tagname> -m "blablabla..."`

用PGP签名标签,`git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；

推送一个本地标签`git push origin <tagname>`

推送全部未推送过的本地标签`git push origin --tags`

删除一个本地标签` git tag -d <tagname>`

删除一个远程标签`git push origin :refs/tags/<tagname>`

##忽略特殊文件

###问题
有些时候，必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件,每次git status都会显示“Untracked files ...”
###解决办法
在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

忽略文件的原则是：
>*  忽略操作系统自动生成的文件，比如缩略图等；
>* 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
>* 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

##配置别名
`git config --global alias.st status`设置st为status的别名

```
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqd05u3pp8j20ca08agnx.jpg)

##在windows下复制版本库
注意路径
```
git clone administrator@192.168.1.13:c:/GitProject/testrepo
```
##git init VS git init –bare 
用`git init`初始化的版本库用户也可以在该目录下执行所有git方面的操作。但别的用户在将更新push上来的时候容易出现冲突。
使用`git init –bare`方法创建一个所谓的裸仓库，之所以叫裸仓库是因为这个仓库只保存git历史提交的版本信息，而**不允许用户在上面进行各种git操作**，如果你硬要操作的话，只会得到下面的错误（”This operation must be run in a work tree”）
这个就是**最好把远端仓库初始化成bare仓库的原因**。

在裸仓库中只能切换分支，不能合并分支
