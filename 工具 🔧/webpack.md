# Webpack 快速入门教程

## 1. 了解 Webpack 相关

### 1.1 什么是 Webpack ？

* Webpack 是一个模块打包器（bundler）。
* 在 Webpack 看来，前端的所有资源文件（js/json/css/img/less/...）都会作为模块处理。
* 它将根据模块的依赖关系进行静态分析，生成对应的静态资源。

1.2 五个核心概念

* Entry：入口起点（entry point）提示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。
* Output：output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。
* Loader：loader 让 webpack 能够去处理那次额非 JavaScript 文件（webpack 自身职能解析 Javascript）。
* Plugins：
* Mode：

