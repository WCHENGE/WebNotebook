# yarn 入门教程

## 简介

Yarn 是 Facebook，Google，Exponent 和 Tilde 开发的一款新的 JavaScript 包管理工具。

它诞生的目的是解决 npm 面临的问题，即：

1. 安装的时候无法保证速度 / 一致性；
2. 安全问题，因为 npm 安装时允许代码；

Yarn 同样是一个从 npm 注册源获取模块的新的 CLI 客户端。

> 注意：通常情况下不建议通过npm进行安装。npm安装是非确定性的，程序包没有签名，并且 npm 除了做了基本的 SHA1 哈希之外不执行任何完整性检查，这给安装系统程序带来了安全风险。
>
> 基于这些原因，强烈建议你通过最适合于你的操作系统的安装方法来安装 yarn。

## 1. 安装

1. **进入[官方下载页面](https://yarnpkg.com/en/docs/install)安装**

2. 最简单的方法是运行：

   ```shell
   npm install -g yarn
   ```

安装成功后即可查看版本：

```shell
yarn --version
```

## 2. 初始化

进入项目目录下并执行 `yarn init`

```shell
yarn init
```

会在根目录下生成一个package.json，与npm类似。

## 3. 添加依赖

1. 添加包：`yarn add [pkg-name]`，会自动安装最新版本，会覆盖指定版本号；
2. 一次性添加多个包：`yarn add [pkg-name1] [pkg-name2]`；
3. 添加指定版本的包：`yarn add [pkg-name]@ver`；
4. 将包更新到指定版本：`yarn upgrade [pkg-name]@ver`；
5. 将包更新到最新版本：`yarn upgrade --latest [pkg-name]`；
6. 删除包：`yarn remove [pkg-name]`；
7. 一次性删除多个包：`yarn remove [pkg-name1] [pkg-name2]`

## 4. Yarn.lock 自动锁定安装包版本

在安装过程中，会自动生成一个 yarn.lock 文件，yarn.lock 会记录你安装的所有大大小小的。yarn.lock 锁定了安装包的精确版本以及所有依赖项，只要你不删除 yarn.lock 文件，再次运行 yarn install 时，会根据其中记录的版本号获取所有依赖包。有了这个文件，你可以确定项目团队的每个成员都安装了精确的软件包版本，部署可以轻松地重现，且没有意外的 bug。你可以把 yarn.lock 提交到本库里，这样其他签出代码并运行 yarn install 时，可以保证大家安装的依赖都是完全一致的。

## 5. yarn 和 npm 命令对比

| NPM                                 | YARN                               | 说明                                    |
| ----------------------------------- | ---------------------------------- | --------------------------------------- |
| `npm init`                          | `yarn init`                        | 初始化某个项目                          |
| `npm install`                       | `yarn` 、`yarn install`            | 默认安装依赖操作                        |
| `npm install [pkg-name] --save`     | `yarn add [pkg-name] --save` \| -S | 安装某个项目依赖（dependencies）        |
| `npm install [pkg-name] --save-dev` | `yarn add [pkg-name]--dev` \| -D   | 安装某个开发（devDependencies）依赖项目 |
| `npm update [pkg-name] --save`      | `yarn upgrade [pkg-name]`          | 更新某个依赖项目                        |
| `npm uninstall [pkg-name] --save`   | `yarn remove [pkg-name]`           | 移除某个依赖项目                        |
| `npm install [pkg-name] --global`   | `yarn global add [pkg-name]`       | 安装某个全局依赖项目                    |
| `npm run <script>`                  | `yarn run <script>`                | 运行某个命令，可以在 script 脚本中配置  |

