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
git的三个配置文件分别是版本库级别的配置文件， 用户全局级别的配置文件，系统级的配置文件，他们的优先级依次降低

git config -e 打开当前git工作区的.git/config文件进行编辑

git config -e --global 打开用户主目录下的.gitconfig文件进行编辑，如/home/wdl/.gitconfig

git config -e --system 打开系统级配置文件进行编辑，如/etc/gitconfig

读取，修改配置文件中的指定项
读取git config <section>.<key> 如git config core.bare 
修改git config <section>.<key> value 如git config core.bare true
![](/assets/1.png)
向配置文件test.ini中添加配置
GIT_CONFIG=test.ini git config a.b.c.d "hello world"

从配置文件test.ini中读取配置
GIT_CONFIG=test.ini git config a.bc.d
## 是谁完成的提交
如果不设置git全局参数user.name user.email会怎么样？

先清空用户参数

git config --unset --global user.name

git config --unset --global user.email

再查看用户姓名和邮件信息

git config user.name

git config user.email

测试提交：git commit --allow-empty -m "who does commit?" allow-empty允许没有对工作区文件修改时执行提交

此时会发现，git要求设置用户姓名和email信息,再次补全用户姓名和邮箱地址

git commit --amend --allow-empty --reset-author

本地clone 

cd /path/to/my/workspace

git clone algorithm algorithm-step-1

#第二章 GIT暂存区

##修改不能直接提交吗？

git log --stat 可以看到每次提交的文件变更统计

git 工作区，暂存区， 版本库概念：

1. 工作区：就是你在电脑里能看到的目录。

2. 暂存区：英文叫stage, 或index。一般存放在"git目录"下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。

3. 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库, HEAD指针

![](/assets/2.jpg)

echo "Nice to meet you" >> welcome.txt

1. 执行diff操作

`$ git diff

diff --git a/welcome.txt b/welcome.txt

index e69de29..8654372 100644

--- a/welcome.txt

+++ b/welcome.txt

@@ -0,0 +1 @@

+Nice to meet you

`

2. 执行git status -s

 M welcome.txt M前有一个空格

3. 执行git add welcome.txt后再执行git diff

没有输出

4. 执行git diff HEAD

diff --git a/welcome.txt b/welcome.txt

index e69de29..8654372 100644

--- a/welcome.txt

+++ b/welcome.txt

@@ -0,0 +1 @@

+Nice to meet you

5.执行git status -s 

M welcome.txt 

M位于第一列行头，表示版本库中的文件与处于中间任务--提交暂存区(stage ,index)中的文件相比有改动

M位于第二列，表示工作区当前的文件与牌中间任务--提交暂存区（stage, index中的文件相比有改动






























