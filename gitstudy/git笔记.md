git status ：查看状态
git add ：添加到暂存区
git add . : 所有文件添加到暂存区

git commit ：提交到提交区

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

git remote add origin git@github.com:ymtNSN/test.git
origin : 本地仓库和远程仓库地址进行一个关联

git push -u origin master ：推送代码

ssh-keygen -t rsa -C '18516398527@163.com' : 在自己的计算机中增加一个安全的ssh key

git push -u origin dev_ymt 

git clone : 克隆

git pull : 远程拉取代码