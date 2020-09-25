# Git 与 Github

## 第 1 章：Git 的起源

### 1. “自由主义教皇”林纳斯·托瓦兹

* Linux

  > Linus在1991年创建了开源的Linux，从此，Linux系统不断发展，已经成为最大的服务器系统软件了。

* Git 

  > Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！一个月之内，Linux系统的源码已经由Git管理了！

> 详细资料可参考：
>
> * [Git 的诞生](https://www.liaoxuefeng.com/wiki/896043488029600/896202815778784)
> * [Git 简史](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E7%AE%80%E5%8F%B2)
>

### 2.Git 是什么？

* Git 是目前世界上最先进的分布式版本控制（没有之一）。

> 详细资料可参考：
>
> * [ Git 是什么？](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)
> * [Git 简介](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000)
>

## 第 2 章：GIt 的使用

### 1. Git 的安装与配置

#### 1.1 Git 安装

在使用 Git 前我们需要先安装 Git 。Git 目前支持 Linux/Unix、Solaris、Mac、和 Windows 平台上运行。

Git 各平台安装包下载地址为：[http://git-scm.com/downloads](http://git-scm.com/downloads)

#### 1.2 Git 配置

Git 提供了一个叫做 `git config` 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。环境变量可以存放在以下三个不同的地方：

* `/etc/gitconfig` 文件：系统中对所有用户都普遍使用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
* `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
* 当前项目的 Git 目录中的配置文件 （也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。

> 每一个级别的配置都会覆盖上层的相同配置。所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

##### 1.2.1 配置用户信息

配置个人的用户姓名和邮箱地址：

```shell
git config --global user.name "You Name"
git config --global user.email "email@example.com"
```

如果去掉 --global 参数，就只对当前仓库有效。

> 备注：以上两步的名字和邮箱可随意配置，但最好使用自己的邮箱。
>
> *因为 Git 是分布式版本控制系统，所以每个机器都必须自报家门：你的姓名和 Email 地址。*

##### 1.2.2 编辑 git 配置文件

```shell
git config -e # 针对当前仓库
git config -e --global # 针对系统上所有仓库
```

##### 1.2.3 查看配置信息

直接查阅某个环境变量的设定：

* `git config user.name` 用于查看配置的姓名
* `git config user.email` 用于查看配置的邮箱

或要检查已有的配置，可以使用 `git config --list` 命令。

### 2. Git 工作区、暂存区和版本区

先来理解一下 Git 工作区、暂存区和版本库概念：

* 工作区（working Directory）：就是你在电脑里能看到的目录。
* 暂存区（stage）：介于工作区和版本区中间，工作区到版本区的“必经之路”。一般存放在`.git`目录下的 index 文件中 （.git/index），所以我们把暂存区有时也叫作索引（index）。
* 版本区（Repository）：工作区有一个隐藏目录`.git`，准确的来说这个不算工作区，而是 Git 的版本库。

### 3. 创建版本库

版本库又名仓库，英文名 repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被 Git 管理起来，每个文件的修改、删除。Git 都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

创建一个版本库：

* 第一步：创建一个空目录

  ```shell
  mkdir learngit
  cd learngit
  ```

* 第二步：通过`git init`命令把这个目录变成 Git 可以管理的仓库。

  > 瞬间 Git 就把仓库建好了，可以看到当前目录下多了一个`.git`的目录，这个目录是 Git 来跟踪管理版本库的。如果没看到`.git`目录，那是因为这个目录是默认隐藏的，用 `ls -ah`命令就可以看见。

  也可以使用我们指定目录作为 Git 仓库。

  ```shell
  git init newrepo
  ```

把一个文件放到 Git 仓库只需要两步：

* 第一步：git add 把文件添加进暂存区。

  ```shell
  git add readme.txt
  ```

* 第二步：git commit 把暂存区的所有内容提交到当前版本库。

  ```shell
  $ git commit -m "wrote a readme file"
  [master (root-commit) eaadf4e] wrote a readme file
   1 file changed, 2 insertions(+)
   create mode 100644 readme.txt
  ```

  > 简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gir7smqahnj319i0meag6.jpg" style="zoom: 30%;" />

查看文件有没有添加成功，可以通过`git status`命令查看，用`git diff`可以查看修改的内容。

### 4. 差异对比（了解）

* `git diff`：显示暂存区与工作区的差异。

* `git diff --cached [file]`：显示暂存区和上一个 commit 的差异。

* `git diff HEAD`：显示工作区与当前分支最新 commit 之间的差异。

### 5. 日志

* `git log`显示当前分支从最近到最远的所有提交日志。
* `git reflog`显示当前分支的每次提交（commit）的 commit id。

### 6. 版本回退 + 版本穿梭 + 版本撤销

* `git reset --hard HEAD^`  版本回退（回退一次提交）。

  > 在 git 中，用 `HEAD` 表示当前版本，回退到上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`（最多只能出现两个 `^^` ）

* `git reset --hard <commit_id> ` 回退到指定 commit_id 的 commit id 版本。

* `git reset HEAD <file>` 把暂存区的修改撤销掉（unstage），重新放回到工作区。

* `git reset HEAD` 用**版本库**中的文件区替换**暂存区**的全部文件。

* `git checkout -- <file>` 用**暂存区指定文件区**替换**工作区的指定文件**。<span style="color:red">（危险）</span>

  > 命令`git checkout -- <file>`意思就是把 file 文件在工作区的修改全部撤销，这里分两种情况：
  >
  > * 一种是，文件自修改后还没有被放到暂存区，现在撤销修改，就回到了和版本库一模一样的状态；
  > * 另一种是，文件已经添加到暂存区后，又做了修改，现在撤销修改，就回到添加到暂存区后的状态。
  >
  > `git checkout -- <file>`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令。

* `git checkout HEAD <file>` 用**版本库中的文件**替换**暂存区**和**工作区**的文件。<span style="color:red">（危险）</span>

* `git rm --cached <file>` 从**暂存区**删除文件。

* `git rm <file>` 从版本库中删除该文件。

  > 确定要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`。
  >
  > 💡小提示：先手动删除文件，然后使用 `git rm <file>`和`git add <file>`效果是一样的。
  >
  > ⚠️注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

### 7. 远程仓库

#### 7.1 添加远程库

* 关联一个远程库，使用命令 `git remote add origin <github:url>`；

  > 如果关联失败，说明本地库已经关联了一个远程库，可以用 `git remote -v` 查看远程库信息，然后删除已有的远程库 `git remote rm origin`。

* 关联后，使用命令 `git push -u origin master` 第一次☝️推送 master 分支的所有内容；

  > 此后每次本地提交，只需要使用命令 `git push origin master` 推送最新修改。

分布式版本系统的最大好处之一，是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作（*而 SVN 在没有联网的时候是拒绝工作的*）。当有网络的时候，再把本地提交推送到远程库完成同步。

> 一个本地库能不能既关联 Github ，又关联 Gitee 呢？
>
> 答案是肯定的，因为 git 本身是分布式版本控制系统，可以同步到另外一个远程库，当然也可以同步到另外两个远程库。
>
> 使用多个远程库时，需要注意的是，git 给远程库起的默认名称是 `origin`，如果有多个远程库，我们需要用不同的名称来标识不同的运程库。
>
> 先关联 GitHub 的远程库：
>
> ```shell
> git remote add github https://github.com/xxx/***.git
> ```
>
> 接着，再关联 Gitee 的远程库：
>
> ```shell
> git remote add gitee https://gitee.com/xxx/***.git
> ```
>
> 用 `git remote -v` 查看关联的远程库信息，可以看到两个远程库：
>
> ```shell
> git remote -v
> gitee https://gitee.com/xxx/***.git (fetch)
> gitee https://gitee.com/xxx/***.git (push)
> github https://github.com/xxx/***.git (fetch)
> github https://github.com/xxx/***.git(push)
> ```
>
> 如果要推送到 GitHub ，使用命令：
>
> ```shell
> git push github master
> ```
>
> 如果要推送到 Gitee ，使用命令：
>
> ```shell
> git push gitee master
> ```
>
> 本地库与多个远程库相互同步的关系：
>
> ```ascii
> ┌─────────┐ ┌─────────┐
> │ GitHub  │ │  Gitee  │
> └─────────┘ └─────────┘
>      ▲           ▲
>      └─────┬─────┘
>            │
>     ┌─────────────┐
>     │ Local Repo  │
>     └─────────────┘
> ```

#### 7.2 从远程库克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用 `git clone` 命令克隆。

```shell
$ git clone git@github.com:michaelliao/gitskills.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```

*Git 支持多种协议，包括 `https`、`ssh`等，但 `ssh` 协议速度最快。*

从远程库 clone 到本地，默认情况下只能看到 `master` 分支：（本地推送的分支如果不推送到远程，对于其他人就是不可见的。）

```shell
$ git branch
* master
```

查看远程库的信息，用 `git remote` 命令：

```shell
$ git remote
origin
```

或者用 `git remote -v` 显示更详细的信息：

```shell
$ git remote -v
origin	https://github.com/xxx/***.git (fetch)
origin	https://github.com/xxx/***.git (push)
```

### 8. 分支管理

#### 8.1 创建与合并分支

首先，我们创建 `dev` 分支，然后切换到 `dev` 分支：

```shell
git checkout -b dev
```

> `git checkout` 命令加上 `-b` 参数，表示创建并切换分支，相当于以下两条命令：
>
> ```shell
> git branch dev
> git checkout dev
> ```

用 `git branch ` 命令查看当前分支，`git branch` 命令会列出所有分支，当前分支前面会标一个`*`号。

现在我们从 `dev` 分支切换回 `master` 分支：

```shell
git checkout master
```

把 `dev` 分支的工作成果合并到 `master` 分支上：

```shell
git merge dev
```

合并完分支后，就可以放心地删除 `dev` 分支：

```shell
git branch -d dev
```

删除完分支后，查看 `branch`，就只剩下`master`分支了：

```shell
$ git branch
* master
```

##### 小结：

* 查看分支：`git branch`
* 创建分支：`git branch <name>`
* 切换分支：`git checkout <name>` 或者 `git switch <name>`
* 创建 + 切换分支：`git checkout -b <name>` 或者 `git switch -c <name>`
* 合并某个分支到当前分支：`git merge <name>`
* 删除分支：`git branch -d <name>`
* 强制删除分支：`git branch -D <name>` (*强行删除一个还未被合并的分支*)

#### 8.2 解决冲突

使用 `git merge` 命令合并两个分支，当 Git 无法自动完成合并分支时，就必须首先解决冲突。解决完冲突后，再提交，合并分支。

解决冲突就是把 Git 合并失败的文件手动编辑成我们希望的内容，再提交。

用 `git log --graph` 命令可以看到分支合并图。

#### 8.3 分支策略

在实际开发中，我们应该按照几个原则进行分支管理：

首先，`master` 分支应该是非常稳定的，仅用来发布新版本。平时干活都在 `dev` 分支上，`dev` 分支是不稳定的，到某个阶段（比如 1.0 版本）的时候，再把 `dev` 分支合并到 `master` 分支上，在 `master` 分支上发布 1.0 版本。

你和你的小伙伴每个人都在 `dev`  分支上干活，每个人都有自己的分支，时不时的往 `dev` 分支上合并就可以了。

所以，团队合作时的分支看起来就像这样：

![分支策略](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

合并分时，加上 `--no--ff` 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 `fast forward` 合并就看不出来曾经做过合并。

如果要强制禁用 `Fast forward` 模式，就需要在 `git merge` 命令上加上 `--no--ff` 参数，又因为本次合并要创建一个新的 commit ，所以还要加上 `-m` 参数，并把 commit 描述写进去。

```shell
git merge --no-ff -m "merge with no-ff" <branch>
```

#### 8.4 Bug 分支

修复 bug 时，一般我们会通过创建新的 bug 分支进行修复，然后合并，最后删除。

当手头工作还没有完成时，需要 `git stash` 命令保存工作现场，然后完成 bug 修复，再 `git stash pop` 命令回到工作现场。

> `git stash list` 命令可以用来查看 stash 保存的内容。
>
> 需要恢复一下 stash 存放内容：
>
> * `git stash apply`：恢复后，stash 内容并不回删除，你需要手动用 `git stash drop` 命令来删除；
> * `git stash pop`：恢复的同时把 stash 内容删了。
>
> 多次使用 stash ，需要恢复的时候，先用 `git stash list` 查看，然后恢复指定的 stash ，命令：`git stash apply stash@{0}`

在 master 分支上修复的 bug ，想要合并到当前 dev 分支，可以用 `git cherry-pick <commit>` 命令，把 bug 提交的修改“复制”到当前分支，从而避免重复劳动。

#### 8.5 多人协同

多人👥协作的工作模式通常是：

1. 首先，试图用 `git push origin <branch-name>` 推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功！

> 在本地创建和远程分支对应的分支，使用 `git checkout -b <branch> origin/<branch>`，本地和远程分支的名称最好一致。如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 `git branch --set-upstream-to <branch-name> origin/<branch-name>`。

#### 8.6 Rebase

* rebase 操作可以把本地未 push 的分叉提交历史整理成直线。
* rebase 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

> rebase 操作的特点：
>
> * 优点：把分叉的提交历史“整理”成一条直线，看上去更直观。
>
> * 缺点：本地的分叉提交已经被修改过了。



### 9. 标签管理

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git 的标签虽然是版本库的快照，但其实它就是指向某个 commit 的指针（*跟分支很像对不对？但是分支可以移动，标签不能移动*），所以，创建和删除标签都是瞬间完成的。

#### 9.1 创建标签

* `git tag <tagname>` 用于新建一个标签，默认为 `HEAD` ，也可以指定一个 commit_id。

  ```shell
  git tag <tagname> <commit_id>
  ```

* `git tag` 查看所有标签。

* `git tag -a <tagname> -m "balabala..." [<commit_id>]` 指定标签信息。

* `git show <tagname>` 查看标签信息。（*标签不是按时间顺序列出，而是按字母排序的*）

  > ⚠️注意：标签总是和某个 commit 挂钩的。如果这个 commit 既出现在 master 分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。

#### 9.2 操作标签

* `git push origin <tagname>` 推送一个本地标签。
* `git push origin --tags` 推送全部未推送过的本地标签。

* `git tag -d <tagname>` 删除一个本地分支。
* `git push origin :refs/tags/<tagname>` 删除一个远程标签。

### 10. 使用 GitHub

* 在 GitHub 上，可以任意 Fork 开源仓库；
* 自己拥有 Fork 后的仓库的读写权限；
* 可以推送 pull request 给官方仓库来贡献代码。

### 11. 自定义 Git

#### 11.1 忽略特殊文件

* 忽略某些文件时，需要在 Git 工作区的根目录下创建一个特殊的 `.gitignore` 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件。

  >[GitHub 在线的各种`.gitignore` 配置文件](https://github.com/github/gitignore)

* `.gitignore` 文件本身要放在版本库里，并且可以对 `.gitnore` 做版本管理。

* 忽略文件的原则：

  1. 忽略操作系统自动生成的文件，比如缩略图等；
  2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 Java 编译生成的 `.class` 文件；
  3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

> 有些时候，你想添加一个文件到 Git，但发现添加不了，原因是这个文件 被`.gitignore` 忽略了。如果你确实想添加该文件，可以用 `-f` 强制添加到 Git ：
>
> ```shell
> git add -f <file>
> ```
>
> 或者你发现，可能是 `.gitigonre` 写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore` 命令检查：
>
> ```shell
> $ git check-ignore -v xxx.class
> .gitnore:3:*.class xxx.class
> ```

#### 11.2 配置别名

如果你想敲 `git st` 就表示 `git status` ，就需要告诉 Git ，以后用 `st` 就表示 `status`：

```shell
git config --global alias.st status
```

`--global` 参数是全局参数，也就是这些命令在这台电脑的所有 Git 仓库下都有用。

#### 11.3 搭建 Git 服务器

[搭建 Git 服务器在线教程](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)



### 提交代码的规范

* 每次提交前：先更新，在提交。
* 敏感时间点，一定及时更新文件。
* 多提交，避免“只关注写代码，不关注提交”的现象。
* 每次提交必须书写清晰明了的提交说明。
* 不要提交不能通过编译的代码。
* 不要提交自己不明白的代码。
* 不要提交本地自动生成的文件、文件夹。

[Git 常用命令清单](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)



## 总结：

### 1. Git 与 SVN 的区别在哪里？

> * git 和 svn 最大的区别在与 git 是分布式的，而 svn 是集中式的。因此我们不能在离线的情况下使用 svn 。如果服务器出现问题，我们就没有办法使用 snv 来提交和管理我们的代码。
> * svn 中的分支是整个版本库复制的一份完整目录，而 git 的分支是指针指向某次提交，因此 git 的分支创建开销更小，并且分支上的变化不会影响到其他人，而 svn 的分支变化会影响到所有人。
> * svn 的指令相对于 git 来说要简单一些，比 git 更容易上手。

   详细资料可以参考：
   [《对比 Git 与 SVN，这篇讲的很易懂》](https://juejin.im/post/5bd95bf4f265da392c5307eb)
   [《GIT 与 SVN 世纪大战》](https://blog.csdn.net/github_33304260/article/details/80171456)
   [《Git 学习小记之分支原理》](https://www.jianshu.com/p/e8ad60710017)

### 2. Git 常用命令

> * `git init` 新建 git 代码库
>
>* `git add <file>` 添加指定文件到暂存区
>
>* `git rm` 删除工作区文件，并将这次删除放入暂存区
>
>* `git commit -m <message>` 提交暂存区到仓库区
>
>* `git branch` 列出所有分支
>
>* `git checkout -b <branch>` 新建一个分支，并切换到该分支
>
>* `git status` 显示有变更的文件

 详细资料可以参考：
   [《常用 Git 命令清单》](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)

### 3. git pull 和 git fetch 的区别

> * git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并。
>
> * git pull 会将远程仓库的变化下载下来，并和当前分支合并。

[《详解 git pull 和 git fetch 的区别》](https://blog.csdn.net/weixin_41975655/article/details/82887273)

### 4. git rebase 和 git merge 的区别

> git merge 和 git rebase 都是用于分支合并，关键在于 commit 记录处理上的不同。
>
> * git merge 会创建一个新的 commit 对象，然后将两个分支以前的 commit 记录都指向这个新的 commit 记录。这种方法会保留之前每个分支的 commit 历史。
> * git rebase 会先找到两个分支的第一个共同的 commit 祖先记录，随后将提取当前分支之后所有的 commit 记录，然后将这个 commit 记录添加到目标分支的最新提交后面。经过这样处理后，两个分支合并后的 commit 记录就变为线性的记录了。

 [《git rebase 和 git merge 的区别》](https://www.jianshu.com/p/f23f72251abc)
 [《git merge 与 git rebase 的区别》](https://blog.csdn.net/liuxiaoheng1992/article/details/79108233)





参考资料：

[Git 教程-廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)

[Git 教程-菜鸟教程](https://www.runoob.com/git/git-tutorial.html)

[SVN + Git 版本控制工具-尚硅谷·张天禹](https://www.bilibili.com/video/BV1BK411K7Yk)