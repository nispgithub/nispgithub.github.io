## 一、引言

> 在单人开发过程中，需要进行版本管理，以利于开发进度的控制；
>
> 在多人开发过程中，不仅需要版本管理，还需要进行多人协同控制。

## 二、介绍

> Git是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目；
>
> Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件；
>
> 官网：https://git-scm.com/

## 三、Git安装

### 下载Git

下载Git https://git-scm.com/downloads

### 安装

> 安装，除了安装位置外，其他一直下一步即可

### 基本配置

安装后，打开cmd

```cmd
git config --global user.name "Your Name"  #用户名
git config --global user.email "email@example.com"  #邮箱
# 查看信息
git config -l 
```

### 测试

> 测试：CMD中执行 ,查看git版本

```cmd
git version  
```

### 四、架构

> 版本库：工作区中有一个隐藏目录 `.git`，这个目录不属于工作区，而是git的 `版本库`，是git管理的所有内容；
>
> 暂存区（stage）：版本库中包含一个临时区域，保存下一步要提交的文件；
>
> 分支：版本库中包含若干分支，提交的文件存储在分支中。

### 五、仓库

> 对应的就是一个目录，这个目录中的所有文件被git管理起来；
>
> 以后会将一个项目的根目录，作为仓库；
>
> 仓库中的每个文件的改动都由git跟踪。

### 本地仓库

#### 新建仓库

> 选择一个目录，执行指令：`git init`

#### 工作区

> 执行`git init`的目录即为工作区，如上例，`D:\repo1`目录即为工作区(不包含`.git`目录)
>
> 所有文件，都首先在工作区新建，然后可以存入仓库(版本库)，进行版本控制。

#### 暂存区

> 暂存区也在`.git`目录内，工作区的文件进入仓库时，要先进入暂存区。

####  分支

> 版本控制，简单说，就是记录文件的诸多版本，分支就是这些版本的最终记录位置。

#### 基本操作

![](pic\git1.png)

git status   查看仓库状态 

git add .    将工作区中的文件全部存入暂存区

git commit -m "这里写提交的描述信息"  作用是将暂存区的文件存入分支，形成一个版本

### 远程仓库

当多人协同开发时，每人都在自己的本地仓库维护版本。

但很重要的一点是，多人之间需要共享代码、合并代码，此时就需要一个远程仓库（公共）。

#### 远程仓库选型

> 有很多远程仓库可以选择，比如 :
>
> - Github(https://github.com/)
> - 码云(https://gitee.com/)
>
> 此两种可以注册自己测试使用，但如果是商业项目，需要更多支持需要付费

##### 命令汇总

| 命令                                   | 描述                                         |
| -------------------------------------- | -------------------------------------------- |
| git remote add 标识名(master) 远程地址 | 本地关联远程仓库                             |
| git push 标识名 master                 | 将本地仓库内容上传到远程仓库                 |
| git pull 标识名 master                 | 从远程仓库下载内容到本地仓库                 |
| git clone 远程地址                     | 将远程仓库复制到本地，并自动形成一个本地仓库 |
| git branch [分支名]                    | 查看[创建]当前仓库的分支                     |
| git checkout 分支名                    | 切换分支                                     |
| git log                                | 查看分支的提交日志                           |
| git merge 分支名                       | 合并分支                                     |

##### 合并冲突

> 两个分支进行合并，但它们含有对同一个文件的修改，则在合并时出现冲突，git无法决断该保留改文件哪个分支的修改。
>
> 冲突内容用<<<<   ==== >>>>做了分割

###### 冲突解决

> 出现冲突后，如要由两个开发人员当面协商，该如何取舍，为冲突文件定义最终内容。
>
> 解决方案：
>
> 1. 保留某一方的，删除另一方的
> 2. 保留双方的
> 3. 但无论如何，要记得删除 <<<< ==== >>>> 这些
> 4. 本质是两人协商为冲突的内容，定制出合理的内容。

**GIT 将A分支的某个commit 合并到 B分支** 

1.先切换到A，git log 查看commitID 

2.再切换到B，git cherry-pick A的commitID

 3.最后git status /git push 即可！

## 六、idea使用git

详见文档

## 七、经典问题

> 在使用https协议做push时，如果曾经使用过码云，但密码有过改动，此时会报错

|                      使用https协议报错                       |
| :----------------------------------------------------------: |
| ![坑1](https://study.code2048.tech/assets/%E5%9D%911-58684f4d.jpg) |

> 解决方案: `控制面板-凭据管理器`删除对应凭证，再次使用时会提示重新输入密码。

|             删除之前的码云凭证，然后重新push即可             |
| :----------------------------------------------------------: |
| ![坑2](https://study.code2048.tech/assets/%E5%9D%912-cdf31714.jpg) |

## 八、git常用命令

git branch 查看本地所有分支

git status 查看当前状态

git commit 提交

git branch -a 查看所有的分支

git branch -r 查看本地所有分支

git commit -am "init" 提交并且加注释

git remote add origin git@192.168.1.119:ndshow

git push origin master 将文件给推到服务器上

git remote show origin 显示远程库origin里的资源

git push origin master:develop

git push origin master:hb-dev 将本地库与服务器上的库进行关联

git checkout --track origin/dev 切换到远程dev分支

git branch -D master develop 删除本地库develop

git checkout -b dev 建立一个新的本地分支dev

git merge origin/dev 将分支dev与当前分支进行合并

git checkout dev 切换到本地dev分支

git remote show 查看远程库

git add .

git rm 文件名(包括路径) 从git中删除指定文件

git clone [git://github.com/schacon/grit.git](git://github.com/schacon/grit.git) 从服务器上将代码给拉下来

git config --list 看所有用户

git ls-files 看已经被提交的

git rm [file name] 删除一个文件

git commit -a 提交当前repos的所有的改变

git add [file name] 添加一个文件到git index

git commit -v 当你用－v参数的时候可以看commit的差异

git commit -m "This is the message describing the commit" 添加commit信息

git commit -a -a是代表add，把所有的change加到git index里然后再commit

git commit -a -v 一般提交命令

git log 看你commit的日志

git diff 查看尚未暂存的更新

git rm a.a 移除文件(从暂存区和工作区中删除)

git rm --cached a.a 移除文件(只从暂存区中删除)

git commit -m "remove" 移除文件(从Git中删除)

git rm -f a.a 强行移除修改后文件(从暂存区和工作区中删除)

git diff --cached 或 $ git diff --staged 查看尚未提交的更新

git stash push 将文件给push到一个临时空间中

git stash pop 将文件从临时空间pop下来

\---------------------------------------------------------

git remote add origin [git@github.com](mailto:git@github.com):username/Hello-World.git

git push origin master 将本地项目给提交到服务器中

\-----------------------------------------------------------

git pull 本地与服务器端同步

\-----------------------------------------------------------------

git push (远程仓库名) (分支名) 将本地分支推送到服务器上去。

git push origin serverfix:awesomebranch

\------------------------------------------------------------------

git fetch 相当于是从远程获取最新版本到本地，不会自动merge

git commit -a -m "log_message" (-a是提交所有改动，-m是加入log信息) 本地修改同步至服务器端 ：

git branch branch_0.1 master 从主分支master创建branch_0.1分支

git branch -m branch_0.1 branch_1.0 将branch_0.1重命名为branch_1.0

git checkout branch_1.0/master 切换到branch_1.0/master分支