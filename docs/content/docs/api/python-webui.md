+++
title = "Python WebUI 说明"
description = "后端与前端的关系与启动方式"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 36
template = "docs/page.html"
[extra]
lead = "Python 通过 aiohttp 提供 REST 服务与静态资源，前端可使用内置或自定义静态目录。"
toc = true
+++

- 默认端口：`19159`。可在 CLI 中使用 `-H/-P` 指定。
- 使用 `--static-dir` 可替换内置 `biliup.web` 的静态页面（或在 `app/` 中本地开发 Next.js 前端）。
- Web 端点详见“Web API”章节。