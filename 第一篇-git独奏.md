# 第一章 git初使化

## 创建版本库及第一次提交

一. 配置当前用户名和邮箱地址：

 git config --global user.name "xxx"

 git config --globel user.email "xxx@xxx.com"

 二. 设置git别名

 sudo git config --system alias.ci commit 或

 git config --global alias.ci commit

 三. 命令行中开户颜色

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

1. **工作区**：就是你在电脑里能看到的目录。

2. **暂存区**：英文叫stage, 或index。一般存放在"git目录"下的index文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。

3. **版本库**：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库, HEAD指针

echo "Nice to meet you" >> welcome.txt

##理解GIT暂存区

一. 执行diff操作

`$ git diff

diff --git a/welcome.txt b/welcome.txt

index e69de29..8654372 100644

--- a/welcome.txt

+++ b/welcome.txt

@@ -0,0 +1 @@

+Nice to meet you`

二. 执行git status -s

 M welcome.txt M前有一个空格

三. 执行git add welcome.txt后再执行git diff

 没有输出

四. 执行git diff HEAD

diff --git a/welcome.txt b/welcome.txt

index e69de29..8654372 100644

--- a/welcome.txt

+++ b/welcome.txt

@@ -0,0 +1 @@

+Nice to meet you

五. 执行git status -s 

M welcome.txt 

**M位于第一列行头，表示版本库中的文件与处于中间任务--提交暂存区(stage ,index)中的文件相比有改动**

**M位于第二列，表示工作区当前的文件与处于中间任务--提交暂存区（stage, index中的文件相比有改动**

六. 继续修改welcome.txt后查看状态

echo "BYE-BYE" >> welcome.txt

git status -s

输出： MM welcome.txt 即现在工作区与暂存区相比文件有改动，暂存区和版本库的最新版本相比文件有改动
![](/assets/3.png)

**git diff 显示工作目录与暂存区(index, stage)文件之间的差异**

**git diff HEAD 显示工作目录与git版本库当前分支最新版本（HEAD）之间的差异** 

**git diff --cached (HEAD)或者git diff --staged (HEAD) 显示暂存区与git版本库当前分支最新版本(HEAD)之间的差异**

**git如何执行工作区状态扫描？**

git根据.git/index文件中记录的(用于跟踪工作区文件)时间戳，文件长度等信息判断工作区文件是否改变，若工作区文件时间戳改变了，说明文件内容可能发生变化，需要打开文件，读取文件内容后与更改前的原始文件比较，最终判断文件是否发生变化，再更新该文件新的时间戳到.git/index中。

  .git/index就是一个包含文件索引的目录树，像一个虚拟的工作区，记录了文件名和文件的状态信息(时间戳和文件长度等)。文件内容都没有存储在其中，而是保存在git对象库，即.git/objects目录中，文件索引建立了文件与对象库中对象实体的对应。

![](/assets/2.jpg)

1. 图中左侧为工作区，右侧为版本库，版本库中标识为index的部分是暂存区，标识为Master的是master分支所代表的目录树

2. HEAD实际中是指向master分支的一个游标，所以图示中的命令中出现HEAD的地方可以用master来替换objects标识的区域为GIT的对象库，实际位于.git/objects目录中。

3. 当对工作区修改的文件执行add操作时，暂存区的目录树会刷新，同时工作区修改的文件内容会被写到对象库中的一个新对象中，而改对象的ID会被记录在暂存区的文件索引中

4. 当执行提交操作commit时，暂存区的目录树会写到版本库中，master分支也做相应更新，即Master最新指向的目录树就是提交时原暂存区的目录树

5. 当执行git reset HEAD时，暂存区的目录树会被重写，会被master分支指向的目录树替换，但是工作区不受影响

6. 当执行git rm --cached <file>时，会直接从暂存区中删除文件，工作区不会改变

7. 当执行git checkout . 或git checkout --<file>时，会打暂存区全部文件或者指定文件替换工作区中的文件，因此这个操作很危险。

8. 当执行git checkout HEAD . 或git checkout HEAD <file>时， 会用HEAD指向的master分支中的全部或指定文件替换暂存区和工作区的文件，因此这个操作更危险。

##git diff魔法

1. 工作区，暂存区和版本库的目录树浏览

**显示版本库中目录树：git ls-tree -l HEAD**

100644 blob 8654372b58c7da093073c8b7d0881ce7a0bad803 17 welcome.txt

-l参数表示显示文件大小

第一列是文件的属性(r,w,x), 第二列表示对象库中是一个blob对象， 第三列表示该文件在对象库中对应的ID， 第四列表示文件大小， 第五列文件名

浏览暂存区中的目录树之前，首先清除工作区当前的改动，通过git clean -fd 命令清除当前工作区中没有加入到版本库中的文件和目录， 然后执行git checkout . ， 和暂存区内容刷新工作区

git clean -fd

git checkout .

**显示暂存区中目录树：git ls-files -l -s**

如果想像显示版本库中目录树一样显示暂存区的目录树，需要先将暂存区目录树写入到GIT对象库

**将暂存区目录树写入GIT对象库中：git write-tree**

显示已保存到GIT对象库中的GIT暂存区目录树：git ls-tree -l 暂存区目录树在GIT对象库中的ID

![](/assets/3.png)

**尽量不要使用git commit -a， 因为这样暂存区就失去了存在的意义**

git stash 


























































