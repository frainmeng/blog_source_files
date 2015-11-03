title: Git 常用命令及使用说明
date: 2015-10-29 17:04:01
tags: Git
categories: 项目管理工具
feature: img/git.jpg
toc: true
---


最近在学习git，下面总结了一些常用的命令，帮助自己记忆。

都是一些简单命令，其中包括一些语法、选项和一些使用的实例。

在使用这些命令的过程中可以作为参考。

<!--more-->

### 配置命令`config`

1.  说明
    
    用于配置本机git的相关属性

1.  语法
    
        git config [选项] 属性名 属性值

2.  选项
    
    *   `--global` ：表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址
    *   `--list`：查看所有配置信息  
        
3.  示例
    
        #设置你的名字和email地址
        $ git config --global user.name "Your Name
        $ git config --global user.email "email@example.com"

### `cd`命令

1.  说明
    
    转换当前目录（类似于linux中的cd命令）

2.  语法
    
        cd 目录名（包括相对目录和觉得目录）

3.  选项
    
    无

4.  示例
        
        cd /d/git_repository

### `mkdir`命令

1.  说明
    
    类似linux中的mkdir命令，创建目录

2.  语法
    
    略

3.  选项
    
    略

4.  示例
    
        mkdir testdir

### `pwd`

1.  说明
    
    类似linux

### `init`命令

1.  说明
    
    将当前目录变成Git可以管理的仓库

2.  语法
    
        git init

3.  选项
    
    略

4.  示例
    
    略

### `add`命令

1.  说明
    
    提交的所有修改放到暂存区

2.  语法
    
        git add file

3.  选项
    
    略

4.  示例
    
        git add readme.txt

### `commit`

1.  说明
    
    暂存区的所有修改提交到分支

2.  语法
    
        git commit [选项]

3.  选项
    
    *   `-m`：为本次添加说明
    *   `-a`：相当于执行先执行add再执行commit

4.  示例
    
        git commit -m "add readme file"


### `status`命令

1.  说明
    
    查看修改的文件

2.  语法
    
    git status

3.  选项
    
    略

4.  示例

        git status

### `diff`命令

1.  说明
    
    比较文件修改内容

2.  语法
    
        git  diff file 

3.  选项

    略

4.  示例
    
        git diff readme.txt

### `log`命令

1.  说明
    
    查看历史记录

2.  语法
        
        git log

3.  选项
    
    *   `--pretty=oneline`：显示commit_id（长） + 提交时的说明
    *   `--graph`：显示分支信息
    *   `--abbrev-commit`：显示commit_id + 提交说明
    *   `-num`：num表示数字，查看最近提交

4.  示例
        
        ##查看日志
        git log 
        ##查看日志，显示commit_id content
        git log --pretty=oneline
        ##查看日志，显示commit_id content，并显示分支提交信息
        git log --graph --pretty=oneline --abbrev-commit
        ##查看最近一次提交
        git log --graph --pretty=oneline --abbrev-commit -1
        ##不说了，查看日志用这个就够了
        git log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

### `reset`版本回退命令

1.  说明
    
    将文件的版本回退到以前的版本

2.  语法
    
        git reset [选项] [HEAD^ or commit_id]

        `HEAD`表示当前版本，`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本，一次类推，也可以使用`HEAD~n`表示上n个版本
        其中commit_id为每次提交时生成的id，该id会在每次提交时显示也可以使用`reflog`命令来查看

3.  选项
        
    *   `--hard ` ：略 

4.  示例
        
        ##退回到上上个版本
        git reset --hard HEAD^^
        ##把暂存区的file的修改撤销掉（unstage），重新放回工作区
        git reset HEAD file

### `reflog`

1.  说明
    
    显示你的历史命令

2.  语法
        
        git reflog

3.  选项
    
    略

4.  示例
        
        git reflog

### `checkout`

1.  说明

    *   用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”
    *   从版本库中检出文件或分支
    *   切换分支

2.  语法
    
        git checkout [选项] [file or 分支名称] [远程分支名称]

3.  选项
        
        *   `--`：不切换到另一个分支
        *   `-b`：创建并切换分支    
            相当于如下两个命令：

                git branch dev
                git checkout dev

4.  示例
        
        ##从版本库中检出文件覆盖到工作区
        git checkout -- git常用命令.enmd
        ##从主分支中创建并切换分支
        git checkout -b dev
        ##从主分支中创建并切换分支，同时关联到远程仓库origin的dev分支
        git checkout -b dev origin/dev
        ##切换到master分支
        git checkout master

### `branch`

1.  说明
    
    分支管理命令

2.  语法
    
    git branch [选项] [分支名称]

3.  选项
    
    *   `-a`：查看本地及远程仓库中的分支
    *   `-d`：删除分支
    *   `-D`：强行删除分支
    *   `--set-upstream-to=origin/dev`：将远程仓库的分支与本地仓库的分支管理

4.  示例
    
        ##创建dev分支
        git branch dev
        ##将本地的dev分支与远程仓库origin的dev分支关联
        git branch --set-upstream-to=origin/dev dev
        ##列出所有分支，当前分支前面会标一个*号
        git branch

### `merge`

1.  说明
    
    分支合并

2.  语法
    
    git merge [选项] 分支名称

3.  选项
    
    *   ` --no-ff`：禁用Fast forward
    *   `-m`：在禁用Fast forward，

4.  示例

        git merge dev

### `stash`

1.  说明
    
    暂存当前工作现场

2.  语法
    
        git stash [选项][子命令]

3.  选项
    
    略

4.  **子命令**
    
    *   `list`：查看存储的工作现场
    *   `apply [stash_id]`：恢复工作现场，但是恢复后，stash内容并不删除
    *   `drop [stash_id]`：删除已存储的工作现场
    *   `pop [stash_id]`：恢复工作现场，并删除stash

5.  示例
        
        ##保存当前工作现场
        git stash
        ##查看已保存的工作现场
        git stash list 
        ##恢复现场（单个现场是使用）
        git stash apply
        ##恢复指定的现场
        git stash apply stash@{0}
        ##删除保存的现场
        git stash drop
        ##删除已保存的指定的现场
        git stash drop stash@{0}
        ##恢复现场并从暂存空间删除
        git stash pop
        ##恢复指定的现场并从暂存空间删除
        git stash pop stash@{0}
        
    
### `rm`删除命令

1.  说明
    
    将文件从版本库中移除，移除后要commit

2.  语法
        
        git rm file

3.  选项
        
    略

4.  示例
        
        git rm test.txt

### `remote`

1.  说明
    
    查看远程仓库

2.  语法
        
        git remote [选项] [子命令] ["远程仓库的别名"] [远程仓库的url]

3.  选项

    *   `-v`：显示更详细的信息（抓取和推送的url）

4.  **子命令**
    
    *   `add`：添加远程仓库（即关联本地仓库和远程仓库）需要在本地仓库的**主目录中执行该命令**

5.  示例
        
        ##通过ssh方式添加远程仓库
        git remote add origin https://github.com/frainmeng/learn.git
        ##查看远程仓库
        git remote -v

### `push`

1.  说明
    
    把本地库的内容推送到远程    

2.  语法
        
        git push [选项] 远程仓库别名 [本地仓库分支 or 标签名]

3.  选项
    
    *   `-u`：初次推送使用，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令

4.  示例

        ##初次推送master分支到远程仓库
        git push -u origin master
        ##将dev分支推送至远程仓库
        git push origin dev
        ##将本地标签推送至远程仓库
        git push origin v1.0
        ##将本地所有标签推送至远程仓库
        git push origin --tags
        ##将远程仓库中的标签删除
        git push origin :refs/tags/v1.0

### `clone`

1.  说明
    
    从远程仓库克隆到本地仓库

2.  语法
            
        git clone 远程仓库url

3.  选项
        
    略

4.  示例
    
        git clone git@github.com:frainmeng/learn.git

### `tag`标签

1.  说明
    
    标签操作

2.  语法

        git tag [选项] <tagname> [选项] [commit_id]

3.  选项
    
    *   `-a`：配合`-m`使用，附加说明信息
    *   `-m`：配合`-a`使用，附加说明信息
    *   `-d`：删除标签
    *   `-s`：用私钥签名一个标签
4.  示例
        
        ##创建标签
        git tag v1.0
        ##在指定的（提交）位置创建标签
        git tag v0.9 67833a4
        ##查看所有标签
        git tag
        ##为标签附加说明西悉尼
        git tag -a v0.9 -m "说明信息" 67833a4
        ##删除标签
        git tag -d v0.9

### `show`

1.  说明
    
    显示标签的详细信息

2.  语法
    
    略

3.  选项
    
    略

4.  示例
        
        ##查看标签的详细信息
        git show v1.0
