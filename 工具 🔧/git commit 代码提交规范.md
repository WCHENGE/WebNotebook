# 使用Commitizen规范git commit信息



对于代码提交规范，我们通过使用`husky`来监测`git hooks`钩子，通过以下插件完成对应的配置：

1. `commitlint`：用于检测提交的信息
2. `lint-staged`：检查本次修改更新的代码，并自动修复并且可以添加到暂存区
3. `pre-commit`：`git hooks`的钩子，在代码提交前检查代码是否符合规范，不符合规范将不可被提交
4. `commit-msg`：`git hooks`的钩子，在代码提交前检查`commit`信息是否符合规范
5. `commitizen`：`git`的规范化提交工具，帮助你填写`commit`信息，符合[约定式提交](https://link.juejin.cn?target=https%3A%2F%2Fwww.conventionalcommits.org%2Fzh-hans%2Fv1.0.0%2F)要求

### 提交规范

1. 安装[commitizen](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fcommitizen)

   ```Bash
   pnpm add -D commitizen
   ```

2. 配置项目提交说明，这里我们使用[cz-customizable](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fleoforfree%2Fcz-customizable)，我们先进行安装

   ```Bash
   pnpm add -D node_modules/cz-customizable
   ```

3. 修改`package.json`，代码如下：

   ```JSON
   "scripts": {
       "commit:comment": "引导设置规范化的提交信息",
       "commit":"git-cz",
     },
   
     "config": {
         "commitizen": {
           "path": "node_modules/cz-customizable"
         }
     },
   ```

4. 进行`commit`，通过`cz`这个cli工具

   ```Bash
   npx cz
   ```

   第一步选择本次更新的类型，每个类型的作用如下表所示：

   | Type     | 作用                                                        |
   | -------- | ----------------------------------------------------------- |
   | feat     | 新增特性                                                    |
   | fix      | 修复 Bug                                                    |
   | docs     | 修改文档                                                    |
   | style    | 代码格式修改                                                |
   | refactor | 代码重构                                                    |
   | perf     | 改善性能                                                    |
   | test     | 测试                                                        |
   | build    | 变更项目构建或外部依赖（例如 scopes: webpack、gulp、npm等） |
   | ci       | 更改持续集成软件的配置文件和`package.json`中的`scripts`命令 |
   | chore    | 变更构建流程或辅助工具(比如更改测试环境)                    |
   | revert   | 代码回退                                                    |



### message验证

现在我们定义了提交规范，但是并不能阻止不按照这个规范进行提交，这里我们通过`commitlint`配合`husky`来实现对提交信息的验证规则。

第一步，安装相关依赖

```Bash
pnpm add -D @commitlint/config-conventional @commitlint/cli
```

第二步，创建`commitlint.config.js`配置commitlint

```JavaScript
module.exports = {
  extends: ['@commitlint/config-conventional'],
}
```

> 更多配置项可以参考[官方文档](https://link.juejin.cn?target=https%3A%2F%2Fcommitlint.js.org%2F%23%2Freference-configuration)

第三步，使用`husky`生成`commit-msg`文件，验证提交信息

```Bash
npx husky add .husky/commit-msg "npx --no-install commitlint --edit ${1}"
```

### 自定义交互提示

默认的交互信息是英文的，可以通过`.cz-config.js`文件自定义交互内容

```js
// 在项目根目录添加文件 .cz-config.js
module.exports = {
  types: [
    {
      value: 'feat',
      name: 'feat: ✨ 新功能',
    },
    {
      value: 'style',
      name: 'style: 💎样式美化',
    },
    {
      value: 'perf',
      name: 'perf: 🚀性能优化',
    },
    {
      value: 'fix',
      name: 'fix: 🐛修复bug',
    },
    {
      value: 'build',
      name: 'build: 🛠打包',
    },
    {
      value: 'refactor',
      name: 'refactor: 📦重构',
    },
    {
      value: 'docs',
      name: 'docs: 📚文档变更',
    },
    {
      value: 'test',
      name: 'test: 🚨测试',
    },
    {
      value: 'revert',
      name: 'revert: 🗑回退',
    },
    {
      value: 'ci',
      name: 'ci: ⚙️CI related changes',
    },
  ],
  messages: {
    type: '请选择提交类型(必填)',
    customScope: '请输入文件修改范围(可选)',
    subject: '请简要描述提交(必填)',
    body: '请输入详细描述(可选)',
    breaking: '列出任何BREAKING CHANGES(破坏性修改)(可选)',
    footer: '请输入要关闭的issue(可选)',
    confirmCommit: '确定提交此说明吗？',
  },
  allowCustomScopes: true,
  allowBreakingChanges: ['feat', 'fix'], // 当提交类型为feat、fix时才有破坏性修改选项
  subjectLimit: 100,
}
```

### 使用git cz命令代替git commit

![image-20221101125907117](/Users/wcheng/Library/Application%20Support/typora-user-images/image-20221101125907117.png)