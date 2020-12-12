---
title:
  Git的常见配置和基本使用
tags: [git]
categories: 前端工具
date: 2020/12/11
---

## Git 的安装

- 在 Git 官网下载即可，慢的话选择淘宝镜像来下载；
- 安装好之后存在三种工具：
  - Git Bash ：Unix 和 Linux 风格的命令行，建议使用；
  - Git CMD ：Windows 风格的命令行；
  - Git GUI ：图形界面的 Git，不建议初学者使用；

## 常见的 Linux 命令行

```bash
  $ cd 进入目录
  $ cd .. 返回上一级目录
  $ pwd 显示当前路径
  $ ls 列出当前目录下的所有文件
  $ rm 删除一个文件
  $ rm-r 删除一个文件夹
  $ touch 新建一个文件
  $ mkdir 新建一个目录，即文件夹
  $ clear 清屏（清除bash里的所有历史命令行）
  $ mv 文件 文件夹 将文件移动到文件夹里
  $ history 查看历史命令
  $ exit 退出
```

## Git 的基本配置

### 1.系统配置和用户全局配置

```bash
  # 查看系统config
  $ git config --system --list
  #默认在C:\Program Files\Git\etc\gitconfig，即安装目录下git的etc文件夹里
  # 查看用户全局配置（用户名和Email）
  $ git config --global --list
  #默认在C:\Users\用户\gitconfig
  #如果要删除就直接删除即可，然后重新设置
```

### 2.设置用户名和邮箱（用户标识，必要）

```bash
  #设置名称和邮箱
  $ git config --global user.name "codeweeei" #名称
  $ git config --global user.email 1749894019@qq.com #邮箱
  # 查看用户全局配置（用户名和Email）
  $ git config --global --list
```

## Git 基本理论

### 1.四大区域

- 解释
  - Workspace：工作区，就是平时存放项目代码的地方
  - Index / Stage：暂存区，用于临时存放改动，事实上它只是一个文件，保存即将提交到文件列表信息
  - Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有提交到所有版本的数据。其中 HEAD 指向最新放入仓库的版本
  - Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

### 2.工作流程

- 解释
  - 工作区和暂存区：工作区就是我们电脑能看到的目录，比如 learngit 文件夹就是一个工作区；工作区里的.git 是 Git 的版本库，里面存了暂存区（stage）和自动创建的第一分支（master），以及指向 master 的指针 HEAD；
    第一步是用 git add 把文件添加进去，实际上就是把文件修改添加到暂存区；
    第二步是用 git commit 提交更改，实际上就是把暂存区的所有内容提交到当前分支。

### 3.Git 项目搭建

- 常见命令

  - 掌握仓库的状态：修改 readme.txt 里的内容，使用 $ git status 来查看文件状态（时刻掌握工作区当前的状态）；
  - 查看修改仓库文件修改内容：git diff 比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令。
  - 版本回退：$ git reset --hard HEAD^^ （这里注意使用转义字符^^来表示^），如果很多之间很多版本可以 HEAD~100 回去 100 之前的版本；
    此时使用 git log 命令显示从最近到最远的提交日志时已经没有了回退之前的版本，此时要想回去可以在命令行窗口往上滑找到 append GPL 的 commit id，然后 git reset --hard id 即可回去；
    如果关闭了命令行窗口，也可以命令 git reflog 用来记录你的每一次命令，可以找到之前的 id，同理返回即可；

  - 撤销修改：$ git restore -- readme.txt
    1. 没有 git add 时，用 git restore -- file
    2. 已经 git add 时，先 git reset HEAD file 回退到 1.，再按 1.操作
    3. 已经 git commit 时，用 git reset 回退版本
  - 删除文件：$ git rm 用于删除文件，只要该文件被提交到版本库，那么永远不用担心误删，可以使用 git restore -- test.txt 来恢复到【最新版本】；

### 4.git 分支详解（重点）

- 创建和合并分支：
  - 查看分支：git branch
  - 创建分支：git branch name
  - 切换分支：git checkout name 或者 git switch name
  - 创建+切换分支为当前分支：git checkout -b name 或者 git switch -c name
  - 合并某分支到当前分支：git merge name
  - 删除分支：git branch -d name
- 解决冲突：当 Git 无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。解决冲突就是把 Git 合并失败的文件手动编辑为我们希望的内容，再提交。用 git log --graph 命令可以看到分支合并图。

* 分支管理策略：通常，合并分支时，如果可能，Git 会用 Fast forward 模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用 Fast forward 模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。
  $ git merge --no-ff -m "merge with no-ff" dev 将 dev 分支合并到 master，所以工作时 master 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活，干活都在 dev 分支上，团队合作时创建属于自己的分支，时不时往 dev 分支合并，最终发布版本时就合并到 master 上即可；
* Bug 分支：当软件开发中，每个 bug 都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除，修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；
  当手头工作没有完成时，先把工作现场 git stash 一下，然后去修复 bug，修复后，再 git stash pop，回到工作现场；在 master 分支上修复的 bug，想要合并到当前 dev 分支，可以用 git cherry-pick commit 命令，把 bug 提交的修改“复制”到当前分支，避免重复劳动。
* 删除分支：开发一个新 feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过 git branch -D/d name（强行）删除。
* 多人合作：查看远程库信息，使用 git remote -v；本地新建的分支如果不推送到远程，对其他人就是不可见的；从本地推送分支，使用 git push origin branch-name，如果推送失败，先用 git pull 抓取远程的新提交；
  在本地创建和远程分支对应的分支，使用 git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；建立本地分支和远程分支的关联，使用 git branch --set-upstream branch-name origin/branch-name；
  从远程抓取分支，使用 git pull，如果有冲突，要先处理冲突。

