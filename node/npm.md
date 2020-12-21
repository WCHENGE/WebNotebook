# npm使用教程

## 1.npm配置本地环境

### 1. npm安装和版本更新

[node.js官网安装](https://nodejs.org/en/)

```shell
//当前版本
npm -v
//更新最新版本
npm install npm@latest -g
//测试最新版本
npm install npm@next -g
```

### 2. npm全局配置

node.js 安装，npm 默认路径为 *C:/Users[用户]/administrator[你的计算机名字]/AppData/Roaming/npm*

```shell
//查看配置
npm -g bin
C:\Users\Yuminjun\AppData\Roaming\npm
```

- npm 实际去找全局命令的目录：*C:/Users/[username]/.npmrc* 文件内容的 prefix 值
- npm 包全局 cache 目录：*C:/Users/[username]/.npmrc* 文件内容的 cache 值

```shell
npm config set prefix D:/node/nodejs/node_global/ //全局包目录，就在node安装目录新建了个nodejs文件夹存放
npm config set cache D:/node/nodejs/node_cache/  //全局包缓存目录，就在node安装目录新建了个nodejs文件夹存放
```

### 3. npm初始化一个项目

```shell
// 默认初始化
npm init
npm init --yes
```

配置:

```
name: the current directory name
version: always 1.0.0
description: info from the readme, or an empty string ""
main: always index.js
scripts: by default creates an empty test script
keywords: empty
author: empty
license: ISC
bugs: info from the current directory, if present
homepage: info from the current directory, if present
```

其他初始化

```shell
> npm set init.author.email "15705850186@163.com"
> npm set init.author.name "yuminjun"
> npm set init.license "MIT"

npm get init.license
npm delete init.license
```

**依赖**

```shell
"dependencies"：您的应用程序在生产中需要这些包.
"devDependencies"：这些包只用于开发和测试.

// 添加到 dependencies
npm install <package_name> --save

// 添加到 devDependencies
npm install <package_name> --save-dev
```

### 4.配置查看命令

```shell
npm config ：管理配置文件
npm config set  <key>  <value>[-g|--global] 或 npm set  <key> <value>   [-g|--global] ：设置配置参数
npm config get  <key>或 npm get  <key> ：获取配置参数
npm config delete <key>：删除配置参数
npm config list ：列出配置参数
npm config edit ：直接编辑配置文件
npm config get prefix：获取全局安装包所在地址
```

## 2.npm基础命令

### 安装包

```shell
npm install lodash
npm install lodash@版本号
```

### 更新包

```shell
npm update
```

### 卸载包

```shell
npm uninstall lodash
//从依赖包中删除
npm uninstall --save lodash
```

### 搜索包

```shell
npm search lodash
```

### 发布模块

```shell
npm publish lodash
```

### 撤销发布的代码

```shell
npm unpublish  <package>@<version>
```

### 清空本地缓存

```shell
npm cache clear // 清空npm本地缓存（相同版本号发布新版本时）
```

**安装全局包**

```shell
// 将其作为一个命令行工具，那么你应该将其安装到全局
// -g   全局参数
npm install -g jshint
npm update -g jshint
npm uninstall -g jshint
```

**帮助命令**

```shell
npm [help | -h]  // 查看所有指令
npm [list | -l]  // 查看所有指令使用方法
npm <指令> -h  // 查看具体某条命令的帮助信息
npm help <指令>  // 浏览器打开本地的命令帮助文档
```

**信息查看**

```shell
npm list -g 或 npm ls -g  // 获得全局安装的模块
npm list <package> 或 npm ls <package>  // 查看模块的版本号
npm info <package>  // 显示包的信息
```

**参数**

```shell
-S 或 --save  // 包被写入到 package.json 的 dependencies。
-D 或 --save-dev  // 包被写入到 package.json 的 devDependencies。
-O 或 --save-optional  // 包被写入到 package.json 的 optionalDependencies
-B 或 --save-bundle  // 包也将被添加到 bundleDependencies。
-E 或 --save-exact  // 会在 package.json 文件指定安装模块的确切版本。
```

**npm 设置访问权限**

```shell
npm access public
npm access restricted
```

**npm 浏览器打开 github 网站**

```
npm repo lodash
```

参考文档: [CLI命令](https://docs.npmjs.com/cli-documentation/)

## 3.npm源切换

### 1.安装 nrm

```shell
npm i nrm -g
```

### 2.查看已有源

```shell
nrm ls //查看已有的源

* npm -------- https://registry.npmjs.org/
  yarn ------- https://registry.yarnpkg.com/
  cnpm ------- http://r.cnpmjs.org/
  taobao ----- https://registry.npm.taobao.org/
  nj --------- https://registry.nodejitsu.com/
  npmMirror -- https://skimdb.npmjs.com/registry/
  edunpm ----- http://registry.enpmjs.org/
```

### 3.切换已有源

```shell
nrm use <源名称> //切换到现有的源
```

### 4.新增已有源

```shell
nrm use <源名称> //新增现有的源
```

## 4.npx命令

npx 在npm 5.2.0 是会引入 若无运行命令

```shell
npm install -g npx
```

### 1.npx主要解决调用项目内部模块

```shell
npx eslint --init

node-modules/.bin/eslint --init
```

### 2.避免全局安装模块

```shell
npx create-react-app my-react-app
```

### 3.参数命令

--no-install 强制使用本地

```shell
npx --no-install http-server
```

--ignore-existing 强制使用网络

```shell
npx --ignore-existing create-react-app my-react-app
```

-p 执行多条命令

```shell
npx -p lolcatjs -p cowsay [command]
```

-c 所有命令用npx解释

```shell
npx -p lolcatjs -p cowsay -c 'cowsay hello | lolcatjs'
```

### 3.npx 还可以执行 GitHub 上面的模块源码。

#### 安装包

```shell
npm install lodash
npm install lodash@版本号
```

参考文档:

[npx](http://www.ruanyifeng.com/blog/2019/02/npx.html)

## 5.个人资料管理

### 1.登录

```shell
npm login //登录

npm whoami //检验登录
```

### 2.查看用户配置文件设置

```shell
npm profile get
```

| name            | yuminjun                                          |
| --------------- | ------------------------------------------------- |
| email           | [15705850186@163.com](mailto:15705850186@163.com) |
| two-factor auth | disabled                                          |
| fullname        | yuminjun                                          |
| homepage        |                                                   |
| freenode        |                                                   |
| twitter         |                                                   |
| github          | Men-HuLu                                          |
| created         | 2019-05-24T15:56:54.649Z                          |
| updated         | 2019-06-08T01:56:07.722Z                          |

### 3.修改用户配置文件设置

```shell
npm profile set <prop> <value>
npm profile set fullname ymj
npm profile set password
```

## 6.发布包

### 1.发布包

```shell
//发布前同样要登录
npm login //登录
npm whoami //检验登录

//发布
npm publish 
npm publish --access public
//Scoped包默认为受限制，但您可以将它们公开为使用
//公共发布权限 访问权限为公共
```

### 2.撤销包

// 删除要用 force 强制删除。超过 24 小时就不能删除了。自己把握好时间。

```shell
npm --force unpublish [<@scope>/]<pkg>[@<version>]
```

### 3.初始化范围包

```shell
// 初始化包
// 对于组织范围的包 /my-org  组织名
npm init --scope=@my-org
// 对于用户范围的包 /my-username 用户名
npm init --scope=@my-username
```

npm 包不支持 es6 import 关键字，采用 commonjs 规范，需要采用 babel 转义

## 7.故障修复

### 1.生成日志文件

```shell
npm install --timing
// 对于发布包
npm publish --timing

// npm-debug.log 在 .npm 目录中找到该文件。要查找.npm目录，请使用
npm config get cache
```

### 2.安全审查

```shell
npm audit [--json|--parseable]
npm audit fix [--force|--package-lock-only|--dry-run|--production|--only=dev]
```



## 总结

### 1. npm install、npm install --save 和 npm install --save-dev 的区别？

> **相同点：**
>
> 三者都会将本地安装包安装到项目的 `node_modules`目录中。
>
> **区别：**
>
> 区别在于对项目 package.json 的修改，`npm install` 不会修改 package.json ，而后两者会将依赖添加进 package.json 。
>
> > 指定依赖包：
> >
> > 指定依赖包取决于项目在 package.json 文件中列出需要使用的包，有两种包可以选择：
> >
> > * `dependencies`：这些包都是应用程序在生产环境中所需要的。
> > * `devDependencies`：这些包只是在开发和测试中需要的。

### 1. npm 安装中的 i、-g、--save、--save-dev、-D、-S 的区别？

> **说明：**
>
> * `i ` 是 `install` 的简写
> * `-g` 是全局安装，不带 `-g` 会安装在个人文件夹
> * `-S` 是 `--save` 的简写，安装包信息会写入 `dependencies` 中
> * `-D` 是 `--save-dev` 的简写，安装包写入 `devDependencies` 中
>
> **Dependencies 与 devDependencies：**
>
> * `dependencies` 生产阶段的依赖，也就是项目运行时的依赖
> * `devDependencies` 开发阶段的依赖，就是我们在开发过程中需要的依赖，只在开发阶段起作用
>
> **举例说明：**
>
> > 你写 `ES6` 代码，需要 `babel` 转换成 `es5`，转换完成后，我们只需要转换后的代码，上线的时候，直接把转换后的代码部署到生产环境，不需要 `bebal` 了，生产环境不需要。这就可以安装到 **`devDependencies`** ，再比如说代码提示工具，也可以安装到 **`devDependencies`** 。
> >
> > 如果你用了 `Element-UI`，由于发布到生产后还是依赖 `Element-UI`，这就可以安装到 **`dependencies`** 。

