# Git 与 Github

## 第 1 章：Git 的起源

### 1. “自由注意教皇”林纳斯·托瓦兹

* Linux
* Git 

> 详细资料可参考：
>
> [Git 的诞生](https://www.liaoxuefeng.com/wiki/896043488029600/896202815778784)
>
> [Git 简史](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E7%AE%80%E5%8F%B2)

### 2.Git 是什么？

* Git 是目前世界上最先进的分布式版本控制（没有之一）。

>详细资料可参考：
>
>[ Git 是什么？](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)
>
>[Git 简介](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000)

## 第 2 章：GIt 的使用

### 1. Git 的安装与配置

#### 1.1 Git 安装

在使用 Git 前我们需要先安装 Git 。Git 目前支持 Linux/Unix、Solaris、Mac、和 Windows 平台上运行。

Git 各平台安装包下载地址为：[http://git-scm.com/downloads](http://git-scm.com/downloads)

#### 1.2 Git 配置

Git 提供了一个叫做 `git config` 的工具，专门用来配置或读取相应的工作环境变量。

这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这希望变量可以存放在以下三个不同的地方：

* `/etc/gitconfig` 文件：系统中对所有用户都普遍使用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
* `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
* 当前项目的 Git 目录中的配置文件 （也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置。所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

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

###2. Git 工作区、暂存区和版本区

先来理解一下 Git 工作区、暂存区和版本库概念：

* 工作区（workig Directory）：就是你在电脑里能看到的目录。
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

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gir7smqahnj319i0meag6.jpg" style="zoom: 38%;" />

查看文件有没有添加成功，可以通过`git status`命令查看，用`git diff`可以查看修改的内容。

### 4. 差异对比（了解）

* `git diff`：比较暂存区与工作区。

* `git diff --cached`：比较版本区与暂存区。

* `git diff master`：比较版本区与工作区。

###5. 日志 + 版本号

* `git log`显示从最近到最远的说有提交日志。
* `git reflog`显示每次提交（commit）的 commit id。

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









### 提交代码的规范

* 每次提交前：先更新，在提交。
* 敏感时间点，一定及时更新文件。
* 多提交，避免“只关注写代码，不关注提交”的现象。
* 每次提交必须书写清晰明了的提交说明。
* 不要提交不能通过编译的代码。
* 不要提交自己不明白的代码。
* 慎用锁定功能（尽量避免使用锁，不轻易解锁上锁的文件）。
* 不要提交本地自动生成的文件、文件夹。





参考资料：

[Git 教程-廖雪峰](https://www.liaoxuefeng.com/wiki/896043488029600)

[Git 教程-菜鸟教程](https://www.runoob.com/git/git-tutorial.html)

[SVN + Git 版本控制工具-尚硅谷·张天禹](https://www.bilibili.com/video/BV1BK411K7Yk)