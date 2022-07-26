# pnpm 基本使用



**安装：**

```shell
npm install -g pnpm
```

查看安装`pnpm`版本号：

```shell
pnpm -v
```



**管理依赖：**

* 安装依赖包至`dependencies`：`pnpm add <pkg>`
* 安装依赖包至`devDependencies`：`pnpm add -D <pkg>`
* 安装依赖包至`optionalDependencies`：`pnpm add -O <pkg>`
* 全局安装依赖包：`pnpm add -g <pkg>`
* 安装项目全部依赖：`pnpm install`，别名`pnpm i`
* 更新依赖包：`pnpm update`，别名`pnpm up`
* 删除依赖包：`pnpm remove`，别名`pnpm rm`，`pnpm uninstall`，`pnpm un`



**查看依赖：**

* 运行自定义脚本：`pnpm run xxx`，别名`pnpm xxx`
* 运行`start`启动命令：`pnpm start`
* 启动套件创建项目：`pnpm create`



**管理 node环境：**

可实现`nvm`、`n`等node版本管理工具，安装并切换`node.js`版本的功能：

* 本地安装并使用：`pnpm env use <node版本号>`
* 全局安装并使用：`pnpm env use --global <node版本号>`





**`npm`或`yarn`转`pnpm`：**

1. 全局安装`pnpm`：

   ```shell
   npm install -g pnpm
   ```

2. 删除`npm`或`yarn`生成的`node_modules`：

   ```shell
   rm -rf node_modules
   ```

3. 生成 `pnpm-lock.yaml`：

   ```shell
   pnpm import
   ```

4. 安装依赖：

   ```shell
   pnpm install --frozen-lockfile
   ```

5. 删除`npm`或`yarn`生成的`lock`文件：

   ```shell
   # 删除 package-lock.json
   rm -rf package-lock.json
   
   # 删除 yarn.lock
   rm -rf yarn.lock
   ```