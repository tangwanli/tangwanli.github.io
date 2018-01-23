---
layout: post
title:  "git与github操作说明"
categories: 工具说明
tags: git
excerpt: 这是一篇讲解git命令和github基本操作的文章
---



## 一、git本地操作

#### 1、初始化代码

**注：git命令行窗口中有个master，表示这个是在主分支。要开其它分支，需要用命令来创建。**

> 下面几个代码用来确定git上传到github之后显示的提交者的信息。一般情况，名字和github用户名设置相同
>
> 设置：``git config --global user.name "tangwanli"``
>
> 设置：``git config --global user.email "771147315@qq.com"``
>
> 查看单一信息的话，不加后面的字符串 ``git config --global user.name``
>
> 查看git下面所有的配置 ``git config --list``

#### 2、git3个分区操作

##### 1、文件提交、查看操作

> git中分为3个区：工作区、暂存区、版本库。工作区不能直接将代码提交到版本库，而必须先存到暂存区，再又暂存区提交到版本库。在暂存区的代码也可以取回工作区。

> (1)查看工作区和暂存区的状态`` git status``
>
> (2)添加文件到暂存区 ``git add project/index.js`` 或者添加所有文件到暂存区``git add .``
>
> (3)从暂存区提交到版本库 ``git commit -m "note"``如果后面不加注释，即输入``git commit``之后就直接回车，则会进入一个git bush vim编辑器，这个时候只需要按**两次大写Z**就可以退出。
>
> (4)2、3的简写操作``git commit -a -m "note"``这个实际上是添加所有的文件，即git add .
>
> (5)查看提交的历史``git log``输入之后不会显示全部提交历史，按回车就会再一行一行的输出提交的历史。退出git log就按**q**。后面还可以跟一个-n参数，来固定查看多少条信息。如``git log -2``查看最近的两条提交历史。后面跟文件名，查看这个文件的提交历史``git log pro/try.js``。查看文件夹 下文件提交历史``git log pro/``

##### 2、文件对比操作

> (1)**工作区和暂存区**对比``git diff``.即显示出工作区和暂存区不同的信息。
>
> (2)**暂存区和版本库**对比``git diff --cached``还有一种写法为``git diff --staged``效果完全一样.即显示出暂存区和版本库不同的信息。
>
> (3)**工作区和版本库**对比``git diff master``.即显示出工作区和版本库不同的信息。
>
> (4)**本地版本和fetch下来的线上仓库**对比``git diff master origin/master``.直接显示出本地master和线上master的差别。

##### 3、文件撤销、恢复操作

> (1)把文件从**暂存区回退到工作区**``git reset HEAD``(注：这后面可以不加点，就表示全撤销了。)。撤销单个文件``git reset HEAD try.js``。**把文件区的版本整个回退到版本库中指定的版本**``git reset --hard 459a4049c5f050b0f03b721``。``459a4049c5f050b0f03b721``为commit id。也可以**用HEAD指针**来操作``git reset --hard HEAD^``
>
> **注：HEAD为版本库中的一个指针，指向版本库的最顶层。HEAD^为指针向上移一位。HEAD~n表示指针向上移动n位(n取1，2，3，4....)。**
>
> (2)把文件从工作区回退到版本库``git checkout -- try.js``.即，摧毁工作区当中的那个文件，回退到版本库中那个。**把指定文件回退到指定版本库中文件**，可用``git checkout 459a4049c5f050b0f03b721 try.js``.即回退到``459a4049c5f050b0f03b721``这个版本时候的``try.js``文件.``459a4049c5f050b0f03b721``是用git log查看出来的commit后面的id中的一部分。
>
>  **注：--后面不跟参数而跟空格，表示后面的是一个文件路径，是为了避免歧义。**
>
> (3)在一次commit了之后，把文件提交到了版本库，这个时候就会产生一条log注释。如果要**修改这条log注释**就输入``git commit -m "change the note" --amend``。并且，如果还想要提交文件，和上一次commit的文件一起共享一条注释，则先提交另外的文件，即``git add try.html``，再用刚刚的修改注释命令``git commit -m "change the note" --amend``。
>
> (4)**查看**当前版本库**进行了哪些操作**``git reflog``。运行这个命令就会弹出从clone仓库开始，到现在所进行的所有**关于版本库的操作**(不包括add)。在每一个操作的前面都会有commit id。用这个commit id就可以回到任何版本。

##### 4、文件删除操作

> (1)**只有暂存区有**的文件，**把暂存区的删掉**``git rm try.js``。但是，如果文件在文件区没有被删除，那么这个命令会报错。
>
> (2)文件区和暂存区**都有**的文件，**把两个都删掉**``git rm -f try.js``
>
> (3)文件区和暂存区**都有**的文件，**只删掉暂存区**``git rm --cached try.js``

## 二、远程同步操作

#### 1、同步到远程仓库

> (1)**克隆**远程仓库``git clone git@github.com:tangwanli/my-projects.git``
>
> (2)**查看远程仓库名字**``git remote``。``git remote -v``可以**查看远程仓库的地址**。用``git remote add``可以**修改远程仓库的名字**。
>
> (3)**提交**操作``git push origin master``,origin为远程仓库的名字、master为主分支。如果，要提交到**其他分支**，则先切换到那个分支(比如new1)，然后``git push origin new1``。如果，创建了标签(名字为v1.0)，要**提交标签**``git push origin v1.0``。

#### 2、从远程仓库拉取到本地

> (1)从远程仓库**拉取**过来，但是并**不和本地仓库合并**``git fetch``.再用``git diff master origin/master``查看差别。最后，用``git merge origin/master``进行线上仓库和本地仓库的融合。
>
> (2)从远程仓库**拉取**过来，并且和本地仓库**合并**``git pull``.

#### 3、分支操作

> (1)**查看所有的分支**``git branch``。直接在这后面加个名字就可以**创建分支**，如``git branch new1``,就创建了一个叫new1的分支。
>
> **注：master与new1也是指针，指向最后的那个版本。**
>
> (2)**查看当前分支下合并了哪些分支**,先切换到当前分支，再``git branch --merged``。查看有哪些分支还没有合并在当前分支上``git branch --no-merged``
>
> (3)**切换**到分支``git checkout new1``。即，切换到了new1这个分支上了。快速创建并切换到分支``git checkout -b new2``,就创建了一个new2分支，并且切换到了new2分支上。
>
> (4)分支修改了文件，然后把分支**合并**到主分支上。先切换到主分支，然后``git merge new1``。**如果分支文件和主分支文件有冲突**，即，两个文件修改了同一个地方，是不能直接合并的，这时候用git status可以查看冲突的位置，然后需要我们去**手动修改文件**，才能合并。
>
> (4)**删除**分支。如果，new1合并在主分支上了``git branch -d new1``。如果new1没有合并在主分支上``git branch -D new1``。

#### 4、标签操作

> 注：创建标签即，给自己的仓库打一个版本。
>
> (1)**查看**标签``git tag``。**创建**标签，也是直接加名字``git tag v1.0``


## 三、github操作

##### 1、为自己的仓库添加新的协作人员

> 进入仓库，点击setting再点击collaborators。接下来输入被邀请人的信息就ok了。

##### 2、fork操作

> fork相当于是把开源项目开了一个分支，到自己github里面。然后，clone项目且在自己的本地提交之后(或者直接在自己的github上面进行修改)，是不能直接提交到开源项目上面的，而只是提交到自己仓库上面。如果想要提交到开源项目上，需要**new pull request**再**create pull request**。

##### 3、创建分支

> github上面可以直接创建分支。直接点那个branch，然后输入名字，确定就创建了。

##### 4、创建标签

> github上面可以直接创建标签。直接点那个releases,然后创建就好了。

##### 5、创建组织

> 我们一般都是创建的个人仓库，但是也可以创建组织(组织，即多人开发的一个页面，可以创建多个仓库，可以邀请很多人协同开发。)。直接点new organization,然后就可以创建。

##### 6、博客

> github上面创建博客，就直接是创建一个仓库，然后仓库的名字叫``tanwanli.github.io``.tangwanli为你github的名字。然后，就可以往这个仓库放东西了。
