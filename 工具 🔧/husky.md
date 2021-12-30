#### 在 package.json 目录下

1、执行以下命令：

```shell
npx mrm lint-staged
```

会自动安装 lint-staged 和 husky 并且在 package.json 里写入 lint-staged。

> 注意：mrm 是一个自动化工具，它将根据 package.json 依赖项中的代码质量工具来安装和配置 husky 和 lint-staged，因此请确保在此之前安装并配置所有代码质量工具，如 Prettier 和 ESlint。

如果上面顺利会在 package.json 里写入 lint-staged，可以自行修改让它支持 .vue 文件的校验：

```shell
{
  "lint-staged": {
  	"*.{js,vue}": "eslint --cache --fix"
  }
}
```

可能你在别的地方有看过如下这种在 lint-staged 最后一项加了 git add 的配置，这样也是没问题的。需要说明的是，如果 lint-staged 是 11.x.x 的版本已经自动实现了将修改过的代码自动添加暂存区的功能，所以不需要再加 `git add`。

```shell
"lint-staged": {
   "*.{js,vue}": ["eslint --cache --fix", "git add"]
}
```

2、启动 git hooks

```shell
npx husky install
```

经过上面的命令后，v6 版本的 husky 会在项目根目录新建一个 .husky 目录。如果是 v4 版本的则会写入到 package.json 里。

3、创建 pre-commit 钩子

```shell
npx husky add .husky/pre-commit "npx lint-staged"
```

到这里后，git commit 前自动执行代码校验和修复的功能就算完成了。然后你可以试试修改文件，然后提交试试。