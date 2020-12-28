#常用命令
df 显示磁盘使用情况
du 显示文件系统使用情况

ls 显示目录
普通使用：ls   ls -l   ll
查看隐藏文件：ls -a

cd 切换工作目录
切换到根目录：cd /
切换到上一级目录：cd ..
切换到当前用户家目录： cd
切换到普通用户（cong）家目录： cd  -> cd ~cong (波浪线扩展)

pwd 显示当前工作目录

mkdir 创建目录
普通创建：mkdir abc
建多层次目录：mkdir -p a/b/c

rm 删除
普通删除文件：rm install.log 
强制删除文件：rm -f install.log 
删除文件夹：rm -r -f abc 
删除文件夹 -r和-f两个短参数可以合到一起：rm -rf a 

cp 拷贝
拷贝文件： cp  anaconda-ks.cfg   anaconda-ks-temp.cfg
拷贝文件夹  cp -r a a-temp
