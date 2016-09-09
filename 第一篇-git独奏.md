# 第一章 git初使化

## 创建版本库及第一次提交

 1. 配置当前用户名和邮箱地址：

 git config --global user.name "xxx"

 git config --globel user.email "xxx@xxx.com"

 2. 设置git别名

 sudo git config --system alias.ci commit 或

 git config --global alias.ci commit

 3. 命令行中开户颜色

 git config --global color.ui true

## 为什么根目录下面有一个.git目录

mkdir a/b/c 依层次建立目录
git rev-parse --git-dir 显示版本库.git目录所在位置
git rev-parse --show-toplevel 显示工作区根目录

##git config 命令的各项参数
git config -e 打开当前git工作区的.git/config文件进行编辑
git config -e --global 打开用户主目录下的.gitconfig文件进行编辑，如/home/wdl/.gitconfig
git config -e --system 打开系统级配置文件进行编辑，如/etc/gitconfig






