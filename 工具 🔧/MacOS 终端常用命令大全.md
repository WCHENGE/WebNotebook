# MacOS终端常用命令大全

## 初识终端

![image-20201229100901126](https://tva1.sinaimg.cn/large/0081Kckwgy1gm4i48mbayj31120p0gyu.jpg)

命令的构成：Command Name、Options、Arguments、Extras 四个部分，很多情况下后面三部分都是可省略的。

Options 部分用-作为前导符。其中许多命令的 Options 部分只包含单个字母，这时可以合并。例如:ls -lA和ls -l -A是等效的。Arguments 部分用来细化这个命令或指定这个命令具体的实施对象。Extras 部分则用来进一步实现其他功能。

例子：删除 QQ 这个程序。

```bash
rm -R /Applications/QQ.app
```

## 为什么要使用命令行/如何开启命令行？

许多功能在图形界面不提供，只有通过命令行来实现。 Finder会隐藏许多你不太会需要的文件，然而 command line 会允许你访问所有文件。 通过 command line 可以远程访问你的 Mac（利用 SSH）。 administrators 用户可以通过 sudo命令获得 root 用户权限。 通过 command-line script 可以使工作更高效。 Terminal（终端）程序可以在“实用工具”里找到。 如果你开启手动输入用户名登录模式，登陆时在用户名处输入 >console 可以直接进入命令行界面。随后你仍然需要登录到一个账户。

## 关于 man 命令

不管是mac还是linux都有很多命令，不可能熟练掌握所有命令，即使忘记了使用Google也能查到。mac最强大的一个命令应该算 man xxx ，Mac有上千条命令，每条命令还有许多可选参数和具体的使用方式，但是你却不需要记住这些命令。你只需要记住一个：man，查看具体的命令说明，想要推出直接键入q即可。

大多数命令都会包含一个使用指南，会告诉你任何你需要知道的关于这个命令的所有细节，在命令行中输入 man command-name即可获取。例如，你想知道ls这个命令怎么使用，输入man ls即可进入使用指南页面。使用指南往往很长，所以你可以使用▲（上箭头）或▼（下箭头）来上下移动，使用　来翻页，输入/和关键字来按照关键字搜索，按Q来退出使用指南页面。

那么——如果你连命令名称都不知道怎么办呢？输入man -k和关键字来对整个使用指南数据库进行搜索。

## MacOS 常用终端命令大全：

### 目录操作

| 命令   | 功能描述             | 示例               |
| ------ | -------------------- | ------------------ |
| mkdir  | 创建一个目录         | `mkdir dirname`    |
| rmdir  | 删除一个目录         | `rmdir dirname`    |
| mvdir  | 移动或重命名一个目录 | `mvdir dir1 dir2`  |
| cd     | 改变当前目录         | `cd dirname`       |
| pwd    | 显示当前目录的路径名 | `pwd`              |
| ls     | 显示当前目录的内容   | `ls -la`           |
| dircmp | 比较两个目录的内容   | `dircmp dir1 dir2` |

### 文件操作

| 命令 | 功能描述               | 示例                                    |
| ---- | ---------------------- | --------------------------------------- |
| cat  | 显示或连接文件         | `cat filename`                          |
| pg   | 分页格式化显示文件内容 | `pg filename`                           |
| more | 分屏显示文件内容       | `more filename`                         |
| od   | 显示非文本文件的内容   | `od -c filename`                        |
| cp   | 复制文件或目录         | `cp file1 file2`                        |
| rm   | 删除文件或目录         | `rm filename`                           |
| mv   | 改变文件名或所在目录   | `mv file1 file2`                        |
| ln   | 联接文件               | `ln -s file1 file2`                     |
| find | 使用匹配表达式查找文件 | `find . -name “*.c” -print`             |
| file | 显示文件类型           | `file filename`                         |
| open | 使用默认的程序打开文件 | `open filename` （open . 打开当前目录） |

### 选择操作

| 命令  | 功能描述                       | 示例                           |
| ----- | ------------------------------ | ------------------------------ |
| head  | 显示文件的最初几行             | `head -20 filename`            |
| tail  | 显示文件的最后几行             | `tail -15 filename`            |
| cut   | 显示文件每行中的某些域         | `cut -f1,7 -d: /etc/passwd`    |
| colrm | 从标准输入中删除若干列         | `colrm 8 20 file2`             |
| paste | 横向连接文件                   | `paste file1 file2`            |
| diff  | 比较并显示两个文件的差异       | `diff file1 file2`             |
| sed   | 非交互方式流编辑器             | `sed “s/red/green/g” filename` |
| grep  | 在文件中按模式查找             | `grep “^[a-zA-Z]” filename`    |
| awk   | 在文件中查找并处理模式         | `awk ‘{print 111}’ filename`   |
| sort  | 排序或归并文件                 | `sort -d -f -u file1`          |
| uniq  | 去掉文件中的重复行             | `uniq file1 file2`             |
| comm  | 显示两有序文件的公共和非公共行 | `comm file1 file2`             |
| wc    | 统计文件的字符数、词数和行数   | `wc filename`                  |
| nl    | 给文件加上行号                 | `nl file1 >file2`              |

### 安全操作

| 命令   | 功能描述               | 示例                      |
| ------ | ---------------------- | ------------------------- |
| passwd | 修改用户密码           | `passwd`                  |
| chmod  | 改变文件或目录的权限   | `chmod ug+x filename`     |
| umask  | 定义创建文件的权限掩码 | `umask 027`               |
| chown  | 改变文件或目录的属主   | `chown newowner filename` |
| chgrp  | 改变文件或目录的所属组 | `chgrp staff filename`    |
| xlock  | 给终端上锁             | `xlock -remote`           |

### 编程操作

| 命令  | 功能描述                 | 示例                         |
| ----- | ------------------------ | ---------------------------- |
| make  | 维护可执行程序的最新版本 | `make`                       |
| touch | 更新文件的访问和修改时间 | `touch -m 05202400 filename` |
| dbx   | 命令行界面调试工具       | `dbx a.out`                  |
| xde   | 图形用户界面调试工具     | `xde a.out`                  |

### 进程操作

| 命令   | 功能描述               | 示例               |
| ------ | ---------------------- | ------------------ |
| ps     | 显示进程当前状态       | `ps u`             |
| kill   | 终止进程               | `kill -9 30142`    |
| nice   | 改变待执行命令的优先级 | `nice cc -c *.c`   |
| renice | 改变已运行进程的优先级 | `renice +20 32768` |

### 时间操作

| 命令 | 功能描述                 | 示例         |
| ---- | ------------------------ | ------------ |
| date | 显示系统的当前日期和时间 | `date`       |
| cal  | 显示日历                 | `cal 8 1996` |
| time | 统计程序的执行时间       | `time a.out` |

### 网络与通信操作

| 命令   | 功能描述                          | 示例                          |
| ------ | --------------------------------- | ----------------------------- |
| telnet | 远程登录                          | `telnet www.macwk.com`        |
| rlogin | 远程登录                          | `rlogin hostname -l username` |
| rsh    | 在远程主机执行指定命令            | `rsh f01n03 date`             |
| ftp    | 在本地主机与远程主机之间传输文件  | `ftp ftp.macwk.com`           |
| rcp    | 在本地主机与远程主机 之间复制文件 | `rcp file1 host1:file2`       |
| ping   | 给一个网络主机发送 回应请求       | `ping www.macwk.com`          |
| mail   | 阅读和发送电子邮件                | `mail`                        |
| write  | 给另一用户发送报文                | `write username pts/1`        |
| mesg   | 允许或拒绝接收报文                | `mesg n`                      |

### Korn Shell 命令

| 命令    | 功能描述                        | 示例              |
| ------- | ------------------------------- | ----------------- |
| history | 列出最近执行过的 几条命令及编号 | `history`         |
| r       | 重复执行最近执行过的 某条命令   | `r -2`            |
| alias   | 给某个命令定义别名              | `alias del=rm -i` |
| unalias | 取消对某个别名的定义            | `unalias del`     |

### 其它命令

| 命令   | 功能描述                       | 示例           |
| ------ | ------------------------------ | -------------- |
| uname  | 显示操作系统的有关信息         | `uname -a`     |
| clear  | 清除屏幕或窗口内容             | `clear`        |
| env    | 显示当前所有设置过的环境变量   | `env`          |
| who    | 列出当前登录的所有用户         | `who`          |
| whoami | 显示当前正进行操作的用户名     | `whoami`       |
| tty    | 显示终端或伪终端的名称         | `tty`          |
| stty   | 显示或重置控制键定义           | `stty -a`      |
| du     | 查询磁盘使用情况               | `du -k subdir` |
| df     | 显示文件系统的总空间和可用空间 | `df /tmp`      |
| w      | 显示当前系统活动的总信息       | `w`            |