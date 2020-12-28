#基本命令

1.关机和重启
关机
    shutdown -h now        立刻关机
    shutdown -h 5        5分钟后关机
    poweroff            立刻关机
重启
    shutdown -r now        立刻重启
    shutdown -r 5        5分钟后重启
    reboot                立刻重启
    
2.帮助命令
--help命令
  shutdown --help：
  ifconfig  --help：查看网卡信息
man命令
  man shutdown（q按键退出）
  
#目录操作命令

1.目录切换
命令：cd 目录
cd /        切换到根目录
cd /usr     切换到根目录下的usr目录
cd ../      切换到上一级目录 或者  cd ..
cd ~        切换到home目录
cd -        切换到上次访问的目录

2.目录查看
命令：ls [-al]
ls               查看当前目录下的所有目录和文件
ls -a            查看当前目录下的所有目录和文件（包括隐藏的文件）
ls -l 或 ll      列表查看当前目录下的所有目录和文件
ls /dir          查看指定目录下的所有目录和文件

3.目录操作（增删改查）

1> 增
命令：mkdir 目录
mkdir    aaa            在当前目录下创建一个名为aaa的目录
mkdir    /usr/aaa    在指定目录下创建一个名为aaa的目录

2> 删
删除文件：
rm 文件        删除当前目录下的文件
rm -f 文件     删除当前目录的的文件（不询问）

删除目录：
rm -r aaa    递归删除当前目录下的aaa目录
rm -rf aaa    递归删除当前目录下的aaa目录（不询问）

全部删除：
rm -rf *    将当前目录下的所有目录和文件全部删除
rm -rf /*   【自杀命令！慎用！慎用！慎用！】将根目录下的所有文件全部删除

3> 改
mv a（当前目录）b（新目录）          重命名目录
mv 目录名称 目录的新位置             剪切目录
cp -r 目录名称 目录拷贝的目标位置     拷贝目录

4> 查
find 目录 参数 文件名称

#文件操作命令
