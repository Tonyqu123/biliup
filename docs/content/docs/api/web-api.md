+++
title = "Web API（HTTP 接口）"
description = "后端 aiohttp 服务的 REST 接口"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 35
template = "docs/page.html"
[extra]
lead = "默认监听 0.0.0.0:19159，可通过 CLI 参数自定义。返回 JSON。"
toc = true
+++

基础前缀：无（示例使用 http://localhost:19159）。部分接口位于 `/api` 与 `/v1` 命名空间。

提示：若 CLI 指定了 `--password`，需使用 BasicAuth（用户名 `biliup`）。

### 基础配置

- GET `/api/basic`：获取线路与并发等基础配置
- POST `/api/setbasic`：设置基础配置（含用户 Cookie/Token）
- GET `/api/getconfig`：获取直播间配置（旧）
- GET `/api/save`：保存配置到磁盘，空闲时自动重启

示例：
```bash
curl -s http://localhost:19159/api/basic | jq .
```

### 登录与账号

- GET `/api/login_by_cookie`：使用已有 Cookie 登录（toml 模式下走 stream_gears）
- GET `/v1/get_qrcode`：获取扫码登录二维码数据
- POST `/v1/login_by_qrcode`：提交二维码登录结果，返回 `filename`（cookie 文件路径）
- GET `/v1/users`：列出可用账号（`Configuration` 表内 `bilibili-cookies`）
- POST `/v1/users`：新增账号记录（提交 `platform/value/name`）
- DELETE `/v1/users/{id}`：删除账号

```bash
curl -X POST http://localhost:19159/v1/login_by_qrcode -H 'Content-Type: application/json' -d '{"qrcode":"..."}'
```

### 直播间与历史

- GET `/v1/streamers`：列出直播间（含运行状态 `status`）
- POST `/v1/streamers`：创建直播间（支持 `upload_id` 关联投稿模板）
- PUT `/v1/streamers`：更新直播间
- DELETE `/v1/streamers/{id}`：删除直播间
- GET `/v1/streamer-info`：列出历史会话下载信息（含分段文件）

```bash
curl -s http://localhost:19159/v1/streamers | jq '.[0]'
```

### 投稿模板（UploadStreamers）

- GET `/v1/upload/streamers`：列表
- GET `/v1/upload/streamers/{id}`：详情
- POST `/v1/upload/streamers`：创建（或包含 `id` 时更新）
- PUT `/v1/upload/streamers`：更新
- DELETE `/v1/upload/streamers/{id}`：删除

### 空间配置与导出

- GET `/v1/configuration`：读取 `key=config` 的全局空间配置
- PUT `/v1/configuration`：写入/更新全局空间配置
- POST `/v1/dump`：将当前配置导出到磁盘（提交 `{ path }`）

### 上传任务

- POST `/v1/uploads`：手动发起上传任务
  - 请求体：
```json
{
  "files": [
    { "video": "xxx.mp4", "danmaku": "xxx.xml" }
  ],
  "params": {
    "template_name": "MyTemplate",
    "uploader": "stream_gears",
    "name": "streamer-id"
  }
}
```

### 其它

- GET `/v1/videos`：工作目录下的视频文件列表
- GET `/v1/status`：内核运行状态、版本等
- GET `/url-status`：URL 状态
- GET `/bili/archive/pre`：B 站分区/标签预取
- GET `/bili/space/myinfo?user=<cookiefile>`：读取指定 Cookie 文件关联账号信息
- GET `/bili/proxy?url=<hdslb-url>`：代理获取图片资源（仅允许 *.hdslb.com）

### 错误处理与静态资源

- 404 返回主题内置 `404.html`
- 静态目录 `/static` 仅允许白名单后缀：`.mp4 .flv .3gp .webm .mkv .ts .xml .log`