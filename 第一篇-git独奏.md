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








