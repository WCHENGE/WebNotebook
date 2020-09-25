### 1. Node 是什么？

> Node 是一个 JS 运行环境。

### 2. Node 与 Npm 是什么关系？

> 1. Npm 全称：Node package manager（Node 包管理器），安装了 node 就会自动安装好了 npm。
> 2. 包（库、项目）：电脑上的一个普通文件夹，包含了 package.json，就变成了一个符合 npm 规范的包。
> 3. 使用命令：`npm init` 把一个普通的文件夹变成一个包，即自动生成 package.json。

查看自己 node 的版本命令：`node -v`

查看自己 npm 的版本命令：`npm -v`

### 3. 包名的要求：

* 不能有中文；
* 不能有大些字母；
* 不能与 npm 上已经存在的包重名，例如：（axios、jquery、less等）

### 4. 安装一个包的命令

`npm install xxx`