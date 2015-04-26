title: "Windows上搭建Git服务器"
date: 2015-03-28 16:25:35
category: [开发工具]
tags: [Git]
---
##需要的软件
*   [**msysgit**](http://msysgit.github.io/) (服务器和客户端)

首先你要有一个git环境。git 是分布式版本管理软件，大家都是服务器，大家都是客户端，所以客户端和服务器的安装文件是一样的。
*   [**CopSSH**](https://www.itefix.net/copssh)  (服务器) 

Git支持ssh和http，所以我们要想在git服务器和客户端之间通信通过ssh，就需要再windows server下面安装一个ssh服务器，能够处理ssh请求。COPSSH是一个windows下的SSH服务器,可以远程管理电脑系统。

*   [**TortoiseGit **](http://download.tortoisegit.org/)(客户端) 

TortoiseGit 是git的图形化管理工具。如果对git命令行比较熟悉，可以不用安装。
![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqlipxsfhij20lc07waaj.jpg)
##在Server上安装msygit
下一步，下一步，下一步
##在Server上安装ssh服务器
###安装CopSSH软件
下一步，下一步，下一步
###配置用户
添加系统的administrator用户到SSH服务器当中，之后git需要通过这个用户才能访问。
![enter image description here](http://ww3.sinaimg.cn/mw690/4c2edcb7jw1eqlipy48axj20fn0cadgk.jpg)

![enter image description here](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqlipydgvfj20jm0dg75j.jpg)

设置了密码验证，公钥验证

![enter image description here](http://ww2.sinaimg.cn/mw690/4c2edcb7jw1eqlipympf3j20ji0ddt9z.jpg)

在使用github连接远程仓库的时候我们需要用到命令：
`git remote add origin git@github.com:michaelliao/learngit.git`
`git@github.com`是远程地址，我们连接自建的server要改成`administrator@ipaddress`这样就能够识别远程仓库地址和我们要使用的用户，后面`:path`要改成我们git仓库的地址，稍后有例子。
###配置COPSSH的安装目录
COPSSH获得了ssh请求，如何才能调用git命令呢，需要：
1. 将Git安装目录`yourpath\Git\libexec\git-core`文件夹下的`git-upload-pack.exe`、`git.exe`、`git-receive-pack.exe`和`git-upload-archive.exe`这4个文件复制到SSH的安装路径1yourpath\ICW\bin1下。
2. 将Git安装目录`yourapth\Git\bin\libiconv-2.dll`复制到`yourpath\ICW\bin`下。
 这样COPSSH就可以帮我们调用git命令了
 ![enter image description here](http://ww4.sinaimg.cn/mw690/4c2edcb7jw1eqlipzeknej20s20a4abj.jpg)
 
 ![enter image description here](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqlipznquwj20ir0713z8.jpg)
###测试连接
 本地打开gitbash，ssh连接服务器:`ssh administrator@192.168.199.171`
 输入administrator的密码后就通过ssh远程连接到了服务器上，在这上面可以通过git命令做操作。
##建库操作
使用通过ssh远程登录的账户调用git命令建库

![enter image description here](http://ww1.sinaimg.cn/mw690/4c2edcb7jw1eqlipzviylj20hw05p3z2.jpg)
####关于裸库

用`git init`初始化的版本库用户也可以在该目录下执行所有git方面的操作。但别的用户在将更新push上来的时候容易出现冲突。 
使用`git init –bare`方法创建一个所谓的裸仓库，之所以叫裸仓库是因为这个仓库只保存git历史提交的版本信息，而不允许用户在上面进行各种git操作，如果你硬要操作的话，只会得到下面的错误（”This operation must be run in a work tree”） 
这个就是最好把远端仓库初始化成bare仓库的原因。
在裸仓库中只能切换分支，不能合并分支

##远程clone，push，pull

###clone
在本地打开gitbash，输入
`git clone administrator@192.168.199.171:yourpath/testrepo`

###认证身份
服务器怎么知道clone，push的人的身份呢，COPSSH使用了系统用户的密码，在我们clone的时候，需要输入administrator的密码，这样就完成了身份验证，并不需要把我们本地的公钥加入的COPSSH用户的公钥当中。

```
$ git clone administrator@192.168.199.171:yourpath/testrepo
Cloning into 'testrepo'...
administrator@192.168.199.171's password:
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```
克隆的过程是，克隆过来整个版本库，包括远程的各个分支，在本地创建master分支，把远程的master分支内容克隆到本地的master分支上。
所以我们克隆过来的是master分支，如果我们想获远程取别的分支上的内容，如dev：
`git branch -a`获取所有分支内容
```
$ git branch -a
* (detached from origin/dev)
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
```
`git checkout -b dev remotes/origin/dev`新建dev分支，内容来自`remotes/origin/dev`
###push
`git push origin dev`
推送当前的分支到远程的dev分支
###pull
`git pull origin dev`
含义是获取远程dev分支上的内容到本地工作的分支。
所以也可以通过
1. 在本地创建dev分支
`git checkout -b dev`
2. 拉取远程dev分支的内容
`git pull origin dev`
来获取远程dev分支的内容到本地dev分支
##TortoiseGit 
客户端软件，图形化界面，下一步即可。