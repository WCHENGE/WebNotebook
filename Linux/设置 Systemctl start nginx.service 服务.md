# CentOS7 设置 nginx.service 服务

CentOS7 以上是用 Systemd 进行系统初始化的，Systemd 是 Linux 系统中最新的初始化系统（init），它主要的设计目标是克服 sysvinit 固有的缺点，提高系统的启动速度。

Systemd 服务文件以 .service 结尾，比如现在要建立 nginx 为开机启动，如果用 `yum install`命令安装的，yum 命令会自动创建 nginx.service 文件，直接用命令，设置开机启动即可。

```shell
systemctl enable nginx.service
```

用源码编译安装的，要手动创建 nginx.service 服务文件。

## 1. 在系统服务目录里创建 nginx.service 文件

```shell
cd /usr/lib/systemd/system
vi nginx.service
```

内容如下：

```shell
[Unit]  # 服务的说明
Description=nginx  # 描述服务
After=network.target  # 描述服务类别
  
[Service]  # 服务运行参数的设置
Type=forking  # 后台运行的形式
ExecStart=/usr/local/nginx/sbin/nginx  # 服务的具体运行命令
ExecReload=/usr/local/nginx/sbin/nginx -s reload  # 重启命令
ExecStop=/usr/local/nginx/sbin/nginx -s quit  # 停止命令
PrivateTmp=true  # 表示给服务分配独立的临时空间
  
[Install]  # 运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为 3
WantedBy=multi-user.target
```

重新加载 systemd：

```shell
systemctl daemon-reload
```

> 注意：[Service]的启动、重启、停止命令全部要求使用绝对路径。

## 设置开机启动

```shell
systemctl enable nginx.service
```

## Nginx 常用操作命令

* 设置 nginx 开机自启动：`systemctl enable nginx.service`
* 停止 nginx 开机自启动：`systemctl disable nginx.service`
* 查看 nginx 当前状态：`systemctl status nginx.service`
* 启动 nginx 服务：`systemctl start nginx.service`
* 停止 nginx 服务：`systemctl stop nginx.service`
* 重新启动 nginx 服务：`systemctl restart nginx.service`
* 重新读取 nginx 配置：`systemctl reload nginx.service`
* 查看所有已启动的服务：`systemctl list-units --type=service`

