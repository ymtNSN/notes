git status ：查看状态
git init： 初始化
git add ：添加到暂存区
git add . : 所有文件添加到暂存区

git commit ：提交到提交区

git commit -m 用于提交暂存区的文件；git commit -am 用于提交跟踪过的文件
使用git commit -am 可以省略git add . 这一步，因为git commit -am 可以提交跟踪过的文件

git add命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新的文件,或者把已跟踪的文件放到暂存区，还可以用于合并时把有冲突的文件标记为已解决状态等

git log : 日志
git log --pretty=oneline：日志简化

git reset --hard commitId ：回退到某版本、

git reset --hard HEAD^ ：回退到上一个版本
git reset --hard HEAD^^ : 回退到上上一个版本

git reflog ：进行所有的commitId的查询

git checkout -- <filename> : 撤销修改  未到暂存区时可用

 如果已到暂存区的内容，需要git reset HEAD <filename> 回到提交区中的最新版本，然后再用checkout，将修改拉回到工作区，把工作区的修改内容清空

git checkout -b dev_ymt : 创建一个新分支
git branch -d dev_ymt ： 删除分支

git branch -D dev_ymt : 强制删除分支
git merge dev_ym : 合并分支

git confg -l : 查看所有的信息
git config --global -l : 全局配置

git config --local -l ：本地配置

git config --global --add user.name ymtnsn ： 增加配置项

git config --global --unset user,name :  删除配置项

git config --global alias.st status : 命令配置别名

### 打标签和忽略文件

git tag v1 ： 打上一个标签
git tag : 查看当前仓库的标签
git tag -d v1 : 删除标签

.gitignore : 忽略文件

### 远程仓库

git remote : 查看远程仓库

git remote -v : 远程仓库地址

git remote add origin git@github.com:ymtNSN/test.git
origin : 本地仓库和远程仓库地址进行一个关联

git push -u origin master ：推送代码

ssh-keygen -t rsa -C '18516398527@163.com' : 在自己的计算机中增加一个安全的ssh key

git push -u origin dev_ymt 

git clone : 克隆

git pull : 远程拉取代码

### 比较差异

git diff master origin/master : 比较远程主分支和我的主分支之间的变化

### git push origin master

origin指定了你要push到哪个remote

master其实是一个“refspec”，正常的“refspec”的形式为”+<src>:<dst>”，冒号前表示local branch的名字，冒号后表示remote repository下 branch的名字。注意，如果你省略了<dst>，git就认为你想push到remote repository下和local branch相同名字的branch。听起来有点拗口，再解释下，push是怎么个push法，就是把本地branch指向的commit push到remote repository下的branch，比如

$git push origin master:master (在local repository中找到名字为master的branch，使用它去更新remote repository下名字为master的branch，如果remote repository下不存在名字是master的branch，那么新建一个)

$git push origin master （省略了<dst>，等价于“git push origin master:master”）

$git push origin master:refs/for/mybranch (在local repository中找到名字为master的branch，用他去更新remote repository下面名字为mybranch的branch)

$git push origin HEAD:refs/for/mybranch （HEAD指向当前工作的branch，master不一定指向当前工作的branch，所以我觉得用HEAD还比master好些）

$git push origin :mybranch （再origin repository里面查找mybranch，删除它。用一个空的去更新它，就相当于删除了）

## git stash
1 当正在dev分支上开发某个项目，这时项目中出现一个bug，需要紧急修复，但是正在开发的内容只是完成一半，还不想提交，这时可以用git stash命令将修改的内容保存至堆栈区，然后顺利切换到hotfix分支进行bug修复，修复完成后，再次切回到dev分支，从堆栈中恢复刚刚保存的内容。
2 由于疏忽，本应该在dev分支开发的内容，却在master上进行了开发，需要重新切回到dev分支上进行开发，可以用git stash将内容保存至堆栈中，切回到dev分支后，再次恢复内容即可。
总的来说，git stash命令的作用就是将目前还不想提交的但是已经修改的内容进行保存至堆栈中，后续可以在某个分支上恢复出堆栈中的内容。这也就是说，stash中的内容不仅仅可以恢复到原先开发的分支，也可以恢复到其他任意指定的分支上。git stash作用的范围包括工作区和暂存区中的内容，也就是说没有提交的内容都会保存至堆栈中。

## 1.使用 git fsck --lost-found 命令；
    2.进入.git文件夹中，将lost-found/other ；
    3.git show 《id》 查看

https://www.processon.com/view/link/5fdee6325653bb351c808581