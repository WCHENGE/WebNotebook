# PM2 常用操作命令

## 1. 简介

PM2 是 node 进程管理工具，可以利用它来简化很多 node 应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。

## 2. 不同的 node 守护进程对比

* **nodemon**：开发环境使用，修改后自动重启；
* **forever**：管理多个站点，每个站点的访问量不大，不需要监控；
* **pm2**：网站访问量比较大，需要完整的监控界面。

## 3. pm2 主要特点

* 内建负载均衡（使用 Node cluster 集群模块）
* 后台运行
* 0 秒停机重载
* 开机自启动脚本
* 停止不稳定的进程（避免无限循环）
* 控制台监控
* 提供远程控制和实时的接口 API （允许和 pm2 进程管理器交互）

## 4. pm2 常用命令

### 4.1 安装

```shell
npm install pm2 -g
# 或
wget -qO- https://getpm2.com/install.sh | bash
```

> 若`pm2 -v`不起作用，把 node 目录下 bin 添加到 PATH 路径里。

### 4.2 单个启动

```shell
pm2 start app.js  # 启动
pm2 start app.js -i 4  # 启动4个应用实例，自动负载均衡
pm2 start app.js -i max  # 根据有效CPU数目启动最大进程数目
pm2 start app.js -x  # 用 fork 模式启动 app.js 而不是使用 cluster
pm2 start app.js -x -- -a 23  # 用fork模式启动 app.js并且传递参数（-a 23）
pm2 start app.js --name app_name  # 启动一个进程并把它命名为 app_name

# 监听文件变化，配合 pm2 logs ,方便本地开发
pm2 start app.js --watch
```

> pm2 启动的服务都是在后台运行的，如需部署 docker 上需加 --no-daemon 参数，可通过以下方式实现原生支持。
>
> ```shell
> pm2 start app.js --no-daemon
> # 或
> nohup node app.js
> ```

### 4.3 批量启动

新建 .json 文件，如 server.json ，配置如下：

```json
{
		"apps": [{
    	"name": "appA",
      "script": "./appA.js"
      "watch": false
    }, {
    	"name": "appB",
      "script": "./appB.js",
      "watch": false
    }]
}
```

在执行`pm2 start server.json`。

批量启动是以 restart 模式启动，可以多次执行。

### 4.4 查看

```shell
pm2 list  # 查看进程
pm2 show app_name|app_id  # 查看进程详情
pm2 describe app_name|app_id  # 查看进程详情

pm2 monit  # 监控当前所有的进程，查看CPU和内存资源占用
pm2 monit app_name|app_id  # 监控指定进程

pm2 logs  # 查看日志
pm2 logs app_name|app_id  # 查看进程日志
pm2 logs --json  # JSON output
pm2 logs --format  # Formated output

pm2 flush  # 清除所有日志
pm2 reloadLogs  # 重载所有日志

```

### 4.5 重启

```shell
pm2 restart app_name|app_id  # 重启
pm2 restart all  # 重启所有进程，相当于 stop + start
pm2 reload all  # 0秒停机重载进程（用于不间断进程）
```

### 4.6 停止

```shell
pm2 stop all  # 停止所有
pm2 stop app_name|app_id  # 停止指定进程
```

### 4.7 删除

```shell
pm2 delete app_name|app_id  # 从列表中删除指定的进程
pm2 delete all  # 从列表中删除全部进程
pm2 kill  # 杀死守护进程
```

### 4.8 开机自启动

```shell
pm2 startup  # 创建开机自启动命令
pm2 save  # 保存当前应用列表
pm2 resurrect  # 重新加载保存的应用列表
pm2 unstartup  # 移除开机自启动
```

### 4.9 pm2 更新、移除

```shell
pm2 update  # 更新pm2
npm install pm2@latest -g

pm2 uninstall pm2  # 移除pm2
```

## 5. pm2 启动 next.js

```shell
# for development
pm2 start npm --name "next" -- run dev

# for production
npm run build
pm2 start npm --name "next" -- start
```

## 🔗 相关链接

* pm2 官网：http://pm2.keymetrics.io
* pm2-github：https://github.com/Unitech/pm2