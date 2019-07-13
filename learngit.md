
# 1.创建版本库 ： git init 创建唯一分支(master)

    将文件添加到仓库 ： git add ...                 可以多次加入
    
    将文件提交到版本库 : git commit -m "提交说明/备注"一次提交所有

# 2.时光机穿梭

    git status      查看仓库的当前状态（文件提交到版本库后便无显示）

    git diff   查看具体修改内容。
    

# 3.版本回退
 
     git log (--pretty=oneline) 查看修改内容命令
         
         commit 版本号
         Author 账号名
         Date:  日期
         
   ### 版本回退：
     HEAD表示当前版本，即最新提交的版本
         git reset --hard HEAD^ 回退到上一版本
         git reset --hard 指定版本号前几位数 会退到指定版本
         
         git reflog 记录每一次的git命令
         
     

# 4.工作区和暂存区

    工作区 - git add - 暂存区 - git commit - 版本库 (指针HEAD)
    (Working Directory)(stage/index)    (Repository)  
    
    changes not staged for commit: 没有提交版本库（已经暂存）
    
    Untracked files:   未暂存状态的文件。

# 5.管理修改
    
    git diff HEAD -- 文件名   查看工作区与版本库里最新版本的区别，（当有的文件还没有git add + git commit）

# 6.撤销修改
    
    git checkout -- file 撤销工作区的修改
    
    git reset HEAD file  撤销暂存区的修改（unstage）,重新放回工作区。配合 git chectout -- file 彻底撤销修改
    
    git status 查看修改（工作区、暂存区）

# 7.删除文件
    
    git rm file             
    
        git commit -m ""    彻底删除版本库里的文件
        git checkout -- file 撤销删除 恢复file到最后一次修改的版本库状态。
    
    rm file 后 git rm file 情况下恢复最后的版本库状态：
            git reset HEAD file
            然后 git checkout -- file(会丢失工作区未暂存的修改)

# 8.远程仓库

    1.创建SSH Key
        $ ssh-keygen -t rsa -C "youremail@example.com"
        在用户主目录下生成 id_rsa 和 id_rsa.pub两个文件。(一路回车)
    2.登录github 用户-设置(Account setting)--(SSH Keys)--Add SSH Key:
        Title:任意填
        Key: 上一步id_rsa.pub文件的内容。
    --Add Key

# 9.添加远程库
    
    1.github上新建git仓库 : create a new repo --填入repo name
    --Create repository
    
    2.远程与本地仓库关联
    
        git remote add origin git@github.com:你的github用户名/reponame.git
        origin是默认远程库的名字。
    3.将本地库的所有内容推送到远程库上：
        
        git push (-u) origin master
        
        （-u）参数，在推送（git push）的同时把本地的master分支与远程的master关联起来，在以后推送或拉取时会简化命令。

# 10.从远程库克隆
    
    git clone git@github.com:username/reponamename.git
    
# 11.分支管理

## 1.创建和合并分支
    
    git checkout -b <新的分支名> 创建并切换到新的分支。
    
    git branch  查看当前分支，当前分支前会标一个“×”星号。
    
    git branch <新的分支名>  创建新的分支
    
    git checkout <分支名>    切换到<>分支
    
    git checkout -b <> 等于 git branch <> 加 git checkout <>
    
    git merge <> 将指定分支合并到当前分支。
    
    git branch -d <> 删除指定分支(非当前分支)。
    
    
    

## 2.解决冲突

    git log --graph   查看分支合并图
    
    git status   可以查看冲突

    

## 3.分支管理策略

    git merge --no-ff -m "注释" <目标分支> 禁用 Fast forward
    
    

## 4. Bug分支

    git stash		将工作区“冷冻”起来
    
    git stash 
    git checkout -b <bug分支>
    ...
    git add ...
    git commit -m "..."
    git checkout master
    git merge (--no-ff -m "")<bug分支>
    git branch -d <bug分支>
    
    git stash list 查看 stash 内容存储的地方，其实是看有没有stash内容。
    
    git stash apply 恢复stash内容，但不删除。
    git stash drop  删除stash内容。
    
    两者相加，相当于：
    git stash pop   恢复的同时删除stash内容。
    再用 git stash list 查看，无内容。
    
    
    可以多次 stash,恢复的时候，先用git stash list查看，然后恢复指定的stash:
    			
                git stash apply stash@{0}


## 5.Feature(功能)分支
	    
        丢弃一个还没有合并的分支：
        
        git branch -D <目标分支name>

## 6.多人协作
    因此，多人协作的工作模式通常是这样：

首先，可以试图用git push origin <branch-name>推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。
        