+++
title = "CLI 命令"
description = "biliup 命令行使用说明"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 31
template = "docs/page.html"
[extra]
lead = "通过命令行启动 WebUI、查看版本、控制守护进程。"
toc = true
+++

### 安装

```bash
pip3 install biliup
```

### 用法

```bash
biliup [全局参数] [子命令]
# 或
python3 -m biliup [全局参数] [子命令]
```

### 全局参数

- `--version`: 输出版本号
- `-H <host>`: Web API 监听地址，默认 0.0.0.0
- `-P <port>`: Web API 端口，默认 19159
- `--no-http`: 关闭 Web API（仅运行内核）
- `--static-dir <dir>`: 指定自定义静态目录作为前端 UI
- `--password <pwd>`: 开启 BasicAuth，用户名固定为 `biliup`
- `-v, --verbose`: 增加日志输出（DEBUG）
- `--config <file>`: 指定配置文件（新版本可不提供，推荐使用 WebUI 管理配置）
- `--no-access-log`: 关闭 Web 访问日志

### 子命令（类 Unix 平台）

- `start`: 以守护进程方式启动
- `stop`: 根据 `watch_process.pid` 停止守护进程
- `restart`: 重启守护进程

不带子命令时，将以前台方式启动。

### 示例

- 前台启动并设置 UI 密码：
```bash
biliup --password 'password123'
```

- 指定端口和静态目录：
```bash
biliup -H 0.0.0.0 -P 19159 --static-dir ./public
```

- 后台启动：
```bash
biliup start -P 19159
```

- 停止守护进程：
```bash
biliup stop
```

- 指定配置文件并关闭 Web：
```bash
biliup --config ./config.yaml --no-http
```