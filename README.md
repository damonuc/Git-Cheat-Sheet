# Git Cheat Sheet 中文版
-----------------

<p align="center">
    <img alt="Git" src="./Img/git-logo.png" height="190" width="455">
</p>

------------------

Git cheat sheet 让你不用再去记所有的git命令。

---------------------

# Git Cheat Sheet 中文版
=====================

### 索引
* [配置](#配置)
* [创建](#创建)
* [本地修改](#本地修改)
* [搜索](#搜索)
* [提交历史](#提交历史)
* [分支与标签](#分支与标签)
* [更新与发布](#更新与发布)
* [合并与重置](#合并与重置)
* [撤销](#撤销)
* [Git Flow](#git-flow)

---

## 配置
git的配置文件是三个级别的，分别是repository级别，用户全局级别，系统级别
### 列出当前配置：
```
$ git config --list
```
这里的配置是所有级别的配置

### 列出repository配置：
```
$ git config --local --list
```

### 列出全局配置(针对同一个user)：
```
$ git config --global --list
```

### 列出系统配置(一般不会配置系统级别的配置)：
```
$ git config --system --list
```

### 设置用户名：
```
$ git config --global user.name “[firstname lastname]”
```
如果不加--global, 那就是local的user name, 只针对这个git仓库生效

### 设置用户邮箱：
```
$ git config --global user.email “[valid-email]”
```

### 设置git命令输出为彩色：
```
$ git config --global color.ui auto
```

### 设置git使用的文本编辑器设：
```
$ git config --global core.editor vi
```


### Repository配置对应的配置文件路径[--local]：
```
<repo>/.git/config
```

### 用户全局配置对应的配置文件路径[--global]：
```
~/.gitconfig
```
~/表示当前用户的home目录

### 系统配置对应的配置文件路径[--system]：
```
/etc/gitconfig
```

----------

## Git 基本概念

### 基本逻辑
- 当我们建立一个仓库之后, 会有三个区域, 工作区, 暂存区, 仓库. 工作区就是我们平时看到的文件夹, 暂存区是一个缓存区, 用来存放我们要提交的文件, 仓库是一个文件夹.git, 用来存放我们提交的文件的元数据. 
- 当我们使用git来管理我们文件的时候, 最重要的就是仓库. 
- 工作区是我们平时看到的文件夹, 它相当于一个打开的未保存的文件. 
- 仓库里的文件相当于已经保存的各个版本的文件.
- 暂存区是一个过渡区
- 当我们修改了文件, 我们需要把这个文件add到暂存区, 然后再commit到仓库中. 在commit到仓库之后, 暂存区的文件并不会被删除, 它还是存在的, 只是仓库中的文件和暂存区的文件一样了.
- 一般来说, 当我们add到暂存区, 然后commit到仓库后, 工作区, 暂存区, 仓库的文件是一样的.
  
## 本地仓库基础操作

### 创建一个新的本地仓库:
```
$ git init
```
这个命令把当前目录变为仓库, 会创建一个新的子目录，里面包含一个.git目录，这个.git目录包含了你仓库的所有必须文件，是git用来跟踪仓库的元数据的

也可以指定一个目录
```
$ git init <directory>
```

### 显示工作路径下已修改的文件：
```
$ git status
```

- 列出工作区和暂存区,仓库之间文件的状态(不包括已经放入.gitignore文件中的文件)
- 新建一个文件, 但是还没有add到暂存区, git status会显示为untracked状态
- add到暂存区后，git status会显示为Changes to be committed状态
- commit后，git status会显示为nothing to commit状态
- 暂存区的文件和仓库的文件一样

### 查看工作区的文件
```
$ git ls-files
```
- 默认情况下，git ls-files 列出所有已跟踪的文件，包括已修改但尚未暂存的文件。
- -c 或 --cached：只列出已经通过 git add 添加到暂存区的文件。
- -m 或 --modified：只列出已修改但尚未暂存的文件。
- -o 或 --others：只列出未跟踪的文件。
- -i 或 --ignored：只列出被忽略的文件。
- -u 或 --unmerged：只列出有冲突的文件。
- -s 或 --stage：显示文件的状态信息，包括文件的模式(例如，100644 表示普通文件，100755 表示可执行文件)和 SHA-1 哈希值。
- -t：只列出已跟踪的文件。

### 显示与上次提交版本文件的不同：
```
$ git diff
```

- git diff: (比较工作区和暂存区的差异)当工作区有改动，临时区为空，diff的对比是“**工作区**与**最后一次commit提交的仓库**的共同文件”；当工作区有改动，临时区不为空，diff对比的是“**工作区**与**暂存区**的共同文件”。
- git diff –cached 或 git diff –staged：(比较暂存区和仓库的差异)显示 **暂存区(已add但未commit文件)和最后一次commit(HEAD)** 之间的所有不相同文件的增删改
- git diff HEAD：(比较工作区和版本库的差异)显示 **工作目录(已track但未add文件)和暂存区(已add但未commit文件)** 与最后一次commit之间的的所有不相同文件的增删改
- git diff <分支名1> <分支名2>: 比较两个分支上最后 commit 的内容的差别 
- git diff <版本id> <版本id>: 比较两个 commit 之间的差别(只要填入版本id)
- git diff <版本id> <版本id> <文件地址>: 比较两个 commit 之间的某个具体文件的差别(只要填入版本id)


### 把当前所有修改添加到下次提交中：
```
$ git add .
```
或者把指定文件的修改添加到下次提交中：
```
$ git add <file>
```

指定文件可以使用通配符:
- 比如：git add *.txt
- 比如：git add doc/*.txt
- 比如：git add doc/README.md
- 比如：git add doc/README.md doc/README2.md
- 比如：git add doc/README*
- 比如：git add doc/README?.md
- 比如：git add doc/README[1-9].md
- 比如：git add doc/README{1,2}.md
- 比如：git add doc/README{1,2,3}.md
- 比如：git add doc/README{1..3}.md
- 比如：git add doc/README{1..3}.md doc/README{1..3}.txt
- 比如：git add doc/README{1..3}.{md,txt}


```
$ git add -p <file>
```
git add -p 或 git add --patch 是 Git 的一个交互式命令，用于分阶段地添加文件到暂存区（staging area）。通过这个命令，你可以选择性地添加文件的特定部分，而不是整个文件。

### 提交本地的所有修改：
```
$ git commit -m 'message here'
```
- 每次commit都需要一个message, 用来描述这次提交的内容
- 如果不添加-m参数，会进入vim编辑器，会进入交互模式, 默认是vim编辑器，可以通过配置修改默认编辑器
- 使用i键进入编辑模式，编辑提交信息
- 编辑完成后，按esc键, 回到命令模式，输入:wq，回车，即可提交
- 如果不想提交，可以输入:q!，回车，退出编辑器
- 如果想取消本次提交，可以输入:q，回车，退出编辑器

```
$ git commit -a -m 'message here'
```
- git commit -a加了-a,在 commit 的时候,能帮你省一步 git add ,但也只是对修改和删除文件有效, 新文件还是要 git add,不然就是 untracked 状态. 
- -a -m 可以合并成 -am

```
git commit --date="`date --date='n day ago'`" -am "Commit Message"
```
提交，并将提交时间设置为之前的某个日期

### 修改上次提交
```
$ git commit --amend
```
请勿修改已发布的提交记录!
注意：这里的之前指最近的commit，而且没有push到远程。
修改提交的内容分为2种情况：
- 提交了代码之后,又有新的改动，不想创建两个commit
- 发现一个地方改错了，下次提交时不想保留上一次(提交)的记录

这时就可以使用git commit --amend命令把新的内容添加到之前的commit里面,这个命令没有添加新的提交，而是用新提交取代了原始提交。


```
git commit --amend GIT_COMMITTER_DATE="date" 
```
修改上次提交的committer date


```
git commit --amend --date="date"
```
修改上次提交的author date


### 删除文件：
```
$ git rm <file>
```
同时删除工作区和暂存区的文件, 但是在仓库中还是存在的

```
git rm --cached <file>
```
- 只删除暂存区的文件, 但是在工作区和仓库中还是存在的. 
- 所以如果要从仓库中删除文件, 可以先删除暂存区的文件, 然后commit到仓库中. 
- 这样的话, 工作区的文件还在, 只是仓库中的文件被删除了. 
- 作用是比如我们将一个已经提交到仓库的文件加入到.gitignore文件中, 我们想停止管理这个文件,但是还是需要在工作区中保留. 这时就可以使用这个命令删除暂存区的文件, 然后commit到仓库中, 这样仓库中的文件就被删除了

### 撤销
```
$ git reset HEAD^
```
- 这里默认的参数是--mixed，即将暂存区的文件更新成上一个版本
- 工作区的文件不变
- 暂存区的内容和HEAD^的内容一样

```
$ git reset --hard HEAD^
```
- 放弃工作区下的所有修改
- 工作区和暂存区的文件都被被更新到head指向的仓库的版本(HEAD^代表上一个版本，HEAD ^ ^ 代表上上一个版本，HEAD~100代表上100个版本)
- 会丢失工作区和暂存区的修改

```
$ git reset --soft hash
```
- 只是将HEAD指向的commit版本更新成指定的commit版本(只有仓库改变了)
- 工作区和暂存区的文件不变
- 如果使用--soft参数，即将暂存区和工作区的文件不变，只是将HEAD指向的commit版本更新成指定的commit版本(只有仓库改变了)
- 也可以把HEAD改成commit的hash值，即将暂存区的文件更新成指定的commit版本



### 提交日志
```
$ git log
```
- 从最新提交开始，显示所有的提交记录（显示hash， 作者信息，提交的标题和时间）：
- 查询完, 按q退出

```
$ git log --oneline
```
查看简洁的提交记录（仅显示提交的hash和message）

```
$ git log --author="username"
```
显示某个用户的所有提交

```
$ git log -p <file>
```
显示某个文件的所有修改, -p：显示提交的补丁（具体更改内容）



```
git reflog
```
git所有操作都是可回溯的, 可以使用reflog查看所有操作记录, 找到(想要的, 已经被误操作删了的)对应的commit hash值, 然后使用git reset --hard <版本id> 回到对应的commit版本



---
## 分支

### 列出所有的分支：
```
$ git branch
```

### 基于当前分支创建新分支：
```
$ git branch <new-branch>
```

### 切换分支：
```
$ git checkout <branch>
```


```
$ git checkout -b <branch>
```
创建并切换到新分支

git checkout 还可以用于撤销工作区的修改, 比如
```
$ git checkout -- <file>
```
这个命令会撤销工作区的修改, 如果文件已经提交到暂存区, 工作区的文件会更新成暂存区的文件, 如果文件没有提交到暂存区, 工作区的文件会更新成仓库中的文件


切换分支也可以使用
```
git switch <branch>
```
这个更好, 因为没有歧义. git checkout命令既可以切换分支, 也可以撤销工作区的修改, 所以会产生歧义


### 删除本地分支(分支已合并):
```
$ git branch -d <branch>
```
使用分支的目的是为了修改文件的时候不影响主分支, 但是修改完成后, 当分支合并到主分支后, 可以删除分支
### 强制删除一个本地分支(分支未合并)：

```
$ git branch -D <branch>
```
将会丢失未合并的修改！


### 将分支合并到当前HEAD中：
```
$ git merge <branch>
```

##### 将当前HEAD版本重置到分支中:
<em><sub>请勿重置已发布的提交!</sub></em>
```
$ git rebase <branch>
```
rebase和merge的区别:
- merge是将两个分支的历史合并到一起, 会产生一个新的commit, 会保留两个分支的历史
- rebase是将两个分支的历史合并到一起, 但是会将当前分支的历史放到另一个分支的后面, 会产生一个新的commit, 但是不会保留两个分支的历史
- rebase会产生一个新的commit, 但是不会保留两个分支的历史, 所以rebase会产生一个更加干净的历史
  
### 退出重置:
```
$ git rebase --abort
```

### 解决冲突后继续重置：
```
$ git rebase --continue
```

### 使用配置好的merge tool 解决冲突：
```
$ git mergetool
```

### 在编辑器中手动解决冲突后，标记文件为`已解决冲突`：
```
$ git add <resolved-file>
```

```
$ git rm <resolved-file>
```

### 合并提交：
```
$ git rebase -i <commit-just-before-first>
```

把上面的内容替换为下面的内容：

原内容：
```
pick <commit_id>
pick <commit_id2>
pick <commit_id3>
```

替换为：
```
pick <commit_id>
squash <commit_id2>
squash <commit_id3>
```


## 远程仓库
### 生产SSK Key
- 生成SSH Key
```
$ ssh-keygen -t rsa -b 4096 -C "
```
会生成一个SSH Key, 一般在~/.ssh/目录下，id_rsa是私钥，id_rsa.pub是公钥. ~/是当前用户的home目录
-t rsa: 指定密钥类型为rsa
-b 4096: 指定密钥长度为4096位
-C "email": 添加注释，一般是你的邮箱

- 查看SSH Key
```
$ cat ~/.ssh/id_rsa.pub
```

- 复制SSH Key到剪贴板
```
$ pbcopy < ~/.ssh/id_rsa.pub
```

### 克隆仓库
- 从远程仓库克隆
```
$ git clone repo-url
```

### 推送更新内容
- 推送到远程仓库
```
$ git push origin main
```
origin是远程仓库的别名，可以自定义
main是本地分支的名字

### 拉取更新内容
- 拉取远程仓库的更新
```
$ git pull origin main
```

### 关联本地仓库和远程仓库
- 关联本地仓库和远程仓库
```
$ git remote add origin repo-url
```
origin是远程仓库的别名，可以自定义

- 查看远程仓库
```
$ git remote -v
```

- 关联本地分支和远程分支
```
$ git push -u origin main:main
```
u代表upstream
main:main 代表本地main分支和远程main分支的关联
如果本地分支和远程分支的名字一样，可以简写为
```
$ git push -u origin main
```


- 删除远程仓库
```
$ git remote rm origin
```



### 复制一个已创建的仓库:

```bash
# 通过 SSH
$ git clone ssh://user@domain.com/repo.git
通过
#通过 HTTP
$ git clone http://domain.com/user/repo.git
```
通过HTTP协议clone仓库时，如果仓库是公有的，不需要输入密码，如果是私有的，需要输入用户名和密码



### 列出所有的远端分支：
```
$ git branch -r
```


### 把当前分支中未提交的修改移动到其他分支：
```
git stash
git checkout branch2
git stash pop
```

### 将 stashed changes 应用到当前分支：
```
git stash apply
```

### 删除最新一次的 stashed changes：
```
git stash drop
```



### 基于远程分支创建新的可追溯的分支：
```
$ git branch --track <new-branch> <remote-branch>
```

### 仅显示远端<remote/master>分支与远端<origin/master>分支提交记录的差集：
```
$ git log --oneline <origin/master>..<remote/master> --left-right
```
---

## .gitignore文件
可以忽略某些文件，不让git管理，比如日志文件，编译文件, 配置文件, 密码等
比如在.gitignore文件中添加以下内容：
```
*.log (这代表全部log文件, 也可以指定具体的log文件)
*.key
*.pem
*.p12
*.cer
temp/ (这代表temp文件夹下的所有文件) 
/TODO (这代表当前目录下的Todo文件, 不包括子目录下的Todo文件)
doc/*.txt (这代表doc文件夹下的所有txt文件)
doc/**/*.txt (这代表doc文件夹下的所有txt文件, 包括子目录下的txt文件)
```

匹配规则
- 以斜杠“/”开头表示目录
- 以星号“*”通配多个字符
- 以问号“?”通配单个字符
- 以方括号“[]”包含单个字符的匹配列表
- 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录(取反)
- 从上到下按顺序匹配(下面的规则会覆盖上面的规则)

对于已经添加到仓库的文件，这个时候才添加到.gitignore文件，git仍然会继续管理这些文件，

如果想要忽略这个文件，可以先用git rm --cached <file>从暂存区删除这个文件，然后再commit到仓库中, 现在仓库里没有这个文件里, 并且.gitignore文件里添加了这个文件，git就不会再管理这个文件了
```
git rm --cached <file>
```

### 删除添加`.gitignore`文件前错误提交的文件：
```
$ git rm -r --cached .
$ git add .
$ git commit -m "remove xyz file"
```
- 先从暂存区删除所有文件
- 再重新添加所有文件(此时工作区已经删除了不想要的文件)
- commit到仓库中
  
---

## Git-Flow

### 索引
* [安装](#安装)
* [开始](#开始)
* [特性](#特性)
* [做一个release版本](#做一个release版本)
* [紧急修复](#紧急修复)
* [Commands](#commands)

---

### 安装

- 你需要有一个可以工作的 git 作为前提。
- Git flow 可以工作在 OSX, Linux 和 Windows之下

##### OSX Homebrew:

```
$ brew install git-flow
```

##### OSX Macports:

```
$ port install git-flow
```

##### Linux:

```
$ apt-get install git-flow
```

##### Windows (Cygwin):

安装 git-flow, 你需要 wget 和 util-linux。

```
$ wget -q -O - --no-check-certificate https://github.com/nvie/gitflow/raw/develop/contrib/gitflow-installer.sh | bash
```

----


### 开始

- 为了自定义你的项目，Git flow 需要初始化过程。
- 使用 git-flow，从初始化一个现有的 git 库内开始。
- 初始化，你必须回答几个关于分支的命名约定的问题。建议使用默认值。

```
git flow init
```

---


### 特性

- 为即将发布的版本开发新功能特性。
- 这通常只存在开发者的库中。

##### 创建一个新特性:

下面操作创建了一个新的feature分支，并切换到该分支

```
git flow feature start MYFEATURE
```

##### 完成新特性的开发:

完成开发新特性。这个动作执行下面的操作：
1. 合并 MYFEATURE 分支到 'develop'
2. 删除这个新特性分支
3. 切换回 'develop' 分支

```
git flow feature finish MYFEATURE
```

##### 发布新特性:

你是否合作开发一项新特性？
发布新特性分支到远程服务器，所以，其它用户也可以使用这分支。

```
git flow feature publish MYFEATURE
```

##### 取得一个发布的新特性分支:

取得其它用户发布的新特性分支。

```
git flow feature pull origin MYFEATURE
```

##### 追溯远端上的特性:

通过下面命令追溯远端上的特性

```
git flow feature track MYFEATURE
```

---

### 做一个release版本

- 支持一个新的用于生产环境的发布版本。
- 允许修正小问题，并为发布版本准备元数据。

##### 开始创建release版本:

- 开始创建release版本，使用 git flow release 命令。 
- 'release' 分支的创建基于 'develop' 分支。 
- 你可以选择提供一个 [BASE]参数，即提交记录的 sha-1 hash 值，来开启动 release 分支。
- 这个提交记录的 sha-1 hash 值必须是'develop' 分支下的。 

```
git flow release start RELEASE [BASE]
```

创建 release 分支之后立即发布允许其它用户向这个 release 分支提交内容是个明智的做法。命令十分类似发布新特性：

```
git flow release publish RELEASE
```

(你可以通过 
`git flow release track RELEASE` 命令追溯远端的 release 版本)

##### 完成 release 版本:

完成 release 版本是一个大 git 分支操作。它执行下面几个动作：
1. 归并 release 分支到 'master' 分支。
2. 用 release 分支名打 Tag
3. 归并 release 分支到 'develop'
4. 移除 release 分支。


```
git flow release finish RELEASE
```

不要忘记使用`git push --tags`将tags推送到远端

---


### 紧急修复

紧急修复来自这样的需求：生产环境的版本处于一个不预期状态，需要立即修正。有可能是需要修正 master 分支上某个 TAG 标记的生产版本。

##### 开始 git flow 紧急修复:

像其它 git flow 命令一样, 紧急修复分支开始自：

```
$ git flow hotfix start VERSION [BASENAME]
```


VERSION 参数标记着修正版本。你可以从 `[BASENAME]开始，`[BASENAME]`为finish release时填写的版本号

##### 完成紧急修复:

当完成紧急修复分支，代码归并回 develop 和 master 分支。相应地，master 分支打上修正版本的 TAG。

```
git flow hotfix finish VERSION
```

---


### Commands
<p align="center">
    <img alt="Git" src="./Img/git-flow-commands.png" height="270" width="460">
</p>
<hr>

### Git flow schema

<p align="center">
    <img alt="Git" src="Img/git-flow-commands-without-flow.png">
</p>
<hr>

















---
### 搜索

##### 从当前目录的所有文件中查找文本内容：
```
$ git grep "Hello"
```

##### 在某一版本中搜索文本：
```
$ git grep "Hello" v2.5
```

---

##### 谁，在什么时间，修改了文件的什么内容：
```
$ git blame <file>
```

##### 显示reflog：
```
$ git reflog show 
```

##### 删除reflog：
```
$ git reflog delete
```


##### 给当前版本打标签：
```
$ git tag <tag-name>
```

##### 给当前版本打标签并附加消息：
```
$ git tag -a <tag-name>
```

---
### 更新与发布

##### 列出当前配置的远程端：
```
$ git remote -v
```

##### 显示远程端的信息：
```
$ git remote show <remote>
```

##### 添加新的远程端()：
```
$ git remote add <remote> <url>
```

##### 下载远程端版本，但不合并到HEAD中：
```
$ git fetch <remote>
```

##### 下载远程端版本，并自动与HEAD版本合并：
```
$ git remote pull <remote> <url>
```

##### 将远程端版本合并到本地版本中：
```
$ git pull origin master
```

##### 以rebase方式将远端分支与本地合并：
```
git pull --rebase <remote> <branch>
```

##### 将本地版本发布到远程端：
```
$ git push <remote> <branch>
```

##### 删除远程端分支：
```
$ git push <remote> :<branch> (since Git v1.5.0)
or
git push <remote> --delete <branch> (since Git v1.7.0)
```

##### 发布标签:
```
$ git push --tags
```

---
### 合并与重置(Rebase)


---



##### 放弃某个文件的所有本地修改：
```
$ git checkout HEAD <file>
```

##### 重置一个提交（通过创建一个截然不同的新提交）
```
$ git revert <commit>
```

##### 将HEAD重置到指定的版本，并抛弃该版本之后的所有修改：
```
$ git reset --hard <commit>
```

##### 用远端分支强制覆盖本地分支：
```
git reset --hard <remote/branch> e.g., upstream/master, origin/my-feature
```

##### 将HEAD重置到上一次提交的版本，并将之后的修改标记为未添加到缓存区的修改：
```
$ git reset <commit>
```

##### 将HEAD重置到上一次提交的版本，并保留未提交的本地修改：
```
$ git reset --keep <commit>
```


![20240623203658](https://raw.githubusercontent.com/damonuc/image/main/images/20240623203658.png)