+++
title = "Python 插件架构"
description = "下载/上传插件的编写与注册"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 33
template = "docs/page.html"
[extra]
lead = "通过装饰器注册下载与上传插件，扩展平台与能力。"
toc = true
+++

### 装饰器入口

模块：`biliup.engine.decorators.Plugin`

- `@Plugin.download(regexp)`: 标记并注册一个下载插件类，`regexp` 用于匹配支持的 URL。
- `@Plugin.upload(platform)`: 注册上传插件，键名为 `platform`。
- `Plugin.sorted_checker(urls) -> dict`: 按照 URL 列表返回对应的检测器类映射。
- `Plugin.inspect_checker(url) -> type`: 根据单个 URL 返回匹配的插件类，否则回退到通用插件。

### 下载插件（示例）

```python
from biliup.engine.download import DownloadBase
from biliup.engine.decorators import Plugin

@Plugin.download(r"https?://example.com/(live|room)/\\d+")
class ExampleLive(DownloadBase):
    async def acheck_stream(self, is_check: bool=False) -> bool:
        # 1) 拉取接口，判断是否开播
        # 2) 准备 self.raw_stream_url（真实流地址）
        # 3) 可设置 self.room_title / self.live_cover_url 等
        self.raw_stream_url = "https://cdn.example.com/live/abc123.m3u8"
        return True

    def danmaku_init(self):
        # 可选：初始化弹幕客户端（保存为 .xml）
        pass
```

- 插件可继承 `DownloadBase`，复用 `download()`/`start()` 等流程。
- 若需自定义下载策略，可重写 `download()`。

### 上传插件（示例）

```python
from typing import List
from biliup.engine.upload import UploadBase
from biliup.engine.decorators import Plugin

@Plugin.upload('my-uploader')
class MyUploader(UploadBase):
    def upload(self, file_list: List[UploadBase.FileInfo]) -> List[UploadBase.FileInfo]:
        # 实现上传逻辑；返回成功上传的文件清单
        for file in file_list:
            print('upload', file.video)
        return file_list
```

- 通过设置配置 `uploader: "my-uploader"` 即可切换到自定义上传插件。

### 通用插件与回退

- 当 URL 不匹配任何专用插件时，将回退到 `biliup.plugins.general.__plugin__`，尽量做“通用”处理，但对覆写能力有限制（仅允许修改已存在的属性）。

### 开发建议

- 避免阻塞 IO，网络请求建议使用 httpx/aiohttp 等异步库（结合 `acheck_stream`）。
- 常见功能：
  - 自定义 `fake_headers`、`segment_time`、`file_size`
  - 支持 `stream-gears/ffmpeg/streamlink` 等下载方式
  - 如需封面，设置 `self.live_cover_url`，由框架自动保存
- 配合 UI 的“平台设置”（`/app/ui/plugins`）可提供更丰富的可视化配置项。