Git是目前世界上最先进的分布式版本控制系统

# Git的安装

windows上直接下载安装程序

安装完成后需要配置：

```git
$ git config --globle user.name "your Name"
$ git config --globle user.email "email@example.com"
```

> Git一共有三个级别的config文件，分别是
>
> system ： 整个系统只有一个 配置文件路径：\etc\gitconfig
>
> globle：每个账户有一个 配置文件路径：~/.gitconfig
>
> local ： 每个git仓库一个 配置文件路径：git仓库/.git/config
>
> git commit 的时候，依次读取local、globle和system，如果找到则不再继续读取

# 创建版本仓库

1. git init   初始化仓库

2. git add  <file>  添加文件

3. git commit -m <message> 提交文件，-m 表示注释

#  管理-版本回退

1. git status  查看当前状态

2. git diff <file> 查询修改内容

3. git log  查看历史记录，如果嫌输出的信息太多，使用  git log --pretty=oneline

4. 在Git中，用HEAD表示当前版本，上一个版本就是HEAD^,上上个版本就是HEAD^^, 往上100个版本写成HEAD~100

   git reset --hand HEAD^   回退到上一个版本

5.  git reset --hard <commit id>  指定回退到某个版本， commit id 只写前几位就行了
6. git reflog  记录每一次的git命令，用来查看命令历史

# 工作区和暂存区

工作区： 即当前目录

版本库：工作区有一个隐藏目录 .git ,这个不算工作区，而是Git的版本库。

Git的版本库存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针HEAD。

git add 把文件添加到暂存区，git commit 把暂存区的内容提交到当前分支

# 修改

为什么Git比其他的版本管理系统设计得优秀，因为Git跟踪并管理的是修改，而不是文件

## 撤销修改

1. git checkout -- <file> , 当修改了文件，但是还未提交到暂存区，可以使用 git checkout -- file 命令丢弃工作区的修改
2. git reset HEAD <file>, 将暂存区的文件撤销（unstage），文件放到了工作区。

# 删除文件

1. git rm <file> 删除文件，然后git commit
2. 如果在工作区误删文件， 可以使用 git checkout -- <file> 恢复文件

# 远程仓库

## 添加远程仓库

由于本地Git仓库和Github仓库之间的传输是通过SSH加密的，所以

1. 创建SSH Key ，在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa（私钥） 和 id_rsa.pub（公钥） 这两个文件，如果有了就不用再创建了

   创建SSH Key：

   ```git
   $ ssh-keygen -t rsa -C "youremail@example.com"
   ```

2.  登录Github 在 Account settings -> SSH Keys 中添加 id_rsa.pub 文件中的内容

3. 如何将本地Git仓库和Github上的Git仓库关联？

   首先登录Github，在Create a new repo 创建一个新的仓库

   在本地Git仓库下运行命令：

   ```git
   $ git remote add origin git@github.com:super_mario1990/learngit.git
   ```

   将本地仓库的所有内容推送到远程库上：

   ```git
   $ git push -u origin master
   ```

   git push 命令，将当前分支master推送到远程。

   由于远程是空的，我们第一次推送master分支时，加了-u参数，Git不但会把本地的master分支内容推送到远程的master分支，还会把本地的master 分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令了

   从现在起，只要本地做了提交，就可以通过命令：

   ```git
   $ git push origin master
   ```

# 从远程库克隆

克隆一个本地库：

```git
$ git clone git@github.com:super_mario1990/gitskills.git
```

github给出的地址不止一个，上面走的ssh协议。默认的git://使用ssh，当然可以可用https，只是要慢一些

# 分支管理

