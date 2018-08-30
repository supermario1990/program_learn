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

master：主分支，指向最新的提交

HEAD：指向当前分支

1. 创建分支并切换到分支：

   ```git
   $ git checkout -b <branch>
   ```

   git checkout 命令加上-b参数表示创建并切换，相当于一下两条命令：

   ```git
   $ git branch dev
   $ git checkout dev
   ```

2. 查看当前分支

   ```git
   $ git branch
   ```

3. 在分支上完成工作，就可以切换回master分支：

   ```git
   $ git checkout master
   ```

   将dev分支的工作合并到master分支上：

   ```git
   $ git merge dev
   ```

   当合并完成后，就可以放心的删除dev分支了:

   ```git
   $ git branch -d dev
   ```


# 解决冲突

当Git无法自动合并到分支时，就必须首先解决冲突。解决冲突，再提交。合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用 git log --graph 命令可以看到分支合并图

例如：

```git
$ git log --graph --pretty=oneline --abbrev-commit
```

# 分支管理策略

通常，合并分支时，如果可能，Git会用Fast forward 模式，但是在这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

在合并的时候使用：

```git
$ git merge --no-ff -m "merge with no-ff" dev
```

一般来说在开发中，master都是非常稳定的。干活都在dev分支上，一般的合并向dev分支上合并，等稳定后向master上合并。

# bug分支

当前的dev分支正在进行开发，因为没有完成功能所以不能提交代码。但是发现了一个bug必须要在2小时内修复。这个时候：

```git
$ git stash
```

stash功能，可以把当前工作现场藏起来，等以后恢复现场后继续工作。

1. 首先要确定在哪个分支上修复bug，如果要在master分支上修复，就从master上创建临时分支

   ```git
   $ git checkout master
   $ git checkout -b issue-101
   $ git add readme.txt  # 这一步修复bug
   $ git checkout master
   $ git merge --no-ff -m "merge bug fix 101" issue-101
   ```

2. 然后接着回到dev分支干活

   ```git
   $ git checkout dev
   $ git stash list #查看工作现
   ```

3. 要恢复之前的工作现场

   ```git
   $ git stash apply # 恢复，但是恢复后stash内容并不删除
   $ git stash drop # 删除stash内容
   
   # 另一种方式就是直接使用
   $ git stash pop # 恢复的同时把stash的内容也删了
   ```

# feature分支

添加一个新功能时，你肯定不希望以为一些实验性质的代码，把主分支搞乱了，所以每添加一个新功能，最好新建一个feature分支，在上面开发，合并，最后删除该feature分支

例子：

```git
$ git checkout -b feature-vulcan
$ git add vulcan.py
$ git commit -m "add feature vulcan"
$ git checkout dev # 切回到dev分支
# 接到命令，新功能取消
$ git branch -d feature-vulcan # 因为还没有被合并，所以这一步会失败

#如果要强行删除
$ git branch -D feature-vulcan

```

要强行丢弃一个没有被合并的分支，可以通过 git branch -D <name> 强行删除

# 多人协作

远程仓库的默认名称是origin

1. 查看远程库信息：

   ```git
   $ git remote
   $ git remote -v #显示更详细的信息
   ```

2. 推送分支

   ```git
   $ git push origin master
   ```

   如果要推送其他分支，比如dev：

   ```git
   $ git push origin dev
   ```

3. 是否推送分支？

   - master分支时主分支，要时刻与远程同步
   - dev分支时开发分支，也要与远程同步
   - bug 可以在本地修复，不用推送
   - feature 分支是否推送，取决于你是否和你的小伙伴在上面开发

4. 抓取分支

   模拟一个小伙伴：

   ```git
   $ git clone git@github.com:michaelliao/learngit.git # 从远程clone时，默认情况下只能看到本地的master分支
   # 想在dev分支上开发，就必须创建远程origin的dev分支到本地
   $ git checkout -b dev origin/dev # 这样就可以在dev上继续修改了
   ```

   模拟自己：

   ```git
   $ git add xxx.txt # 如果自己修改了文件
   $ git push origin dev # 这一步会失败，以为别人推送了，和自己有冲突，要先git pull 到本地
   $ git pull # 也会失败，没有指定本地dev和远程origin/dev分支的连接
   $ git branch --set-upstream-to=origin/dev dev
   $ git pull
   # 可能会有冲突，那么手动解决冲突再提交
   $ git commit -m "xxx"
   ```
# rebase
[参考](http://gitbook.liuhui998.com/4_2.html)

# 标签管理

首先需要切换到打标签的分支上：

```git
$ git branch # 查看分支
$ git checkout master # 切换到master分支
$ git tag <tagname> # 打分支
$ git tag # 查看分支
$ git tag <tagname> <commit id>  # 对应某个commit id打tag
$ git show <tagname> # 查看标签信息
$ git tag -a <tagname> -m "commit msg" <commit id> # 创建带说面的标签
```

# 操作标签

```git
$ git tag -d v0.1 #如果标签打错了，可以删除
$ git push origin <tagname> # 推送某个标签到远程
$ git push origin --tags # 一次性推送全部尚未推送到远程的本地标签
# 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除
$ git tag -d v0.9
$ git push origin :refs/tags/v0.9
```

# 使用github

先fork一下别人的仓库，然后从自己的仓库clone，再进行修改。如果希望别人能接受你的修改，推送一个pull request

# 忽略特殊文件

写 .gitignore 文件，并将此文件上传到仓库，进行版本管理

```git
$ git add -f <filename> # 有时候想添加一个文件，发现添加不了，原因就是这个文件被 .gitignore 文件忽略了,实在想添加就用 -f 
$ git check-ignore -v <filename> # 如果觉得 .gitignore 文件写的有问题，就可以用这个命令检查
```



# 配置别名

```git
$ git config --global alias.st staus # status的别名是st
```



