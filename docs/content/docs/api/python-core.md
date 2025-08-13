+++
title = "Python API（核心）"
description = "下载、上传、事件系统与应用入口"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 32
template = "docs/page.html"
[extra]
lead = "biliup 核心 Python API：下载与上传流程、事件系统、装饰器体系。"
toc = true
+++

### downloader

- 函数：`biliup.downloader.download(fname: str, url: str, **kwargs) -> dict`
  - 说明：根据 `url` 匹配合适的下载插件，启动下载并返回一次下载会话的 `stream_info` 字典（含 `name/url/title/date/end_time/live_cover_path/is_download`）。
  - 常用参数（在 `kwargs` 中）：
    - `format`（或 `suffix`）: 目标封装，如 `flv/mp4/ts/mkv`
    - `override`: 覆写插件配置的字典，仅已适配的插件支持全量覆写
    - 其余参数会按插件实例的属性名注入
  - 示例：
```python
from biliup.downloader import download

stream_info = download('my-streamer', 'https://www.huya.com/123456', format='flv')
print(stream_info['title'], stream_info['date'])
```

- 函数：`biliup.downloader.biliup_download(name: str, url: str, kwargs: dict) -> dict`
  - 说明：对旧版配置的兼容封装；会将 `format` 映射到内部 `suffix`，其余参数转交给 `download`。

- 抽象基类：`biliup.engine.download.DownloadBase`
  - 关键属性：`fname/url/suffix/downloader/segment_time/time_range/file_size/raw_stream_url/danmaku`
  - 关键方法：
    - `async acheck_stream(is_check: bool=False) -> bool`: 检测是否开播并准备 `raw_stream_url`
    - `pre_check() -> bool`: 校验时间段等前置条件
    - `download() -> bool`: 执行实际下载（支持 `stream-gears/ffmpeg/streamlink/sync-downloader`）
    - `start() -> dict`: 管理单次会话重试、封面抓取、数据库落库，返回 `stream_info`

### uploader

- 函数：`biliup.uploader.upload(data: dict)`
  - 入参：`data` 至少包含 `name/url/date`，内部会与全局/主播配置合并并格式化标题简介
  - 行为：根据 `uploader` 选择上传插件（默认 `biliup-rs/stream_gears`），调用插件的 `start()`，返回成功上传的文件列表
  - 示例：
```python
from biliup.uploader import upload

result = upload({
  'name': 'my-streamer',
  'url': 'https://www.huya.com/123456',
  'title': '直播标题',
})
print(result)
```

- 函数：`biliup.uploader.biliup_uploader(filelist: list, data: dict)`：直接传入要上传的文件清单，调用上传插件的 `upload(filelist)`。
- 工具：`fmt_title_and_desc(data)`、`custom_fmtstr(string, date, title, streamer, url)`：帮助格式化标题与简介。

- 基类：`biliup.engine.upload.UploadBase`
  - 内嵌类型：`FileInfo(video: str, danmaku: Optional[str])`
  - 静态方法：`file_list(index) -> List[FileInfo]` 扫描待传文件；`remove_filelist(file_list)`、`remove_file(file)` 清理
  - 实例方法：`upload(file_list) -> List[FileInfo]`（由具体上传插件实现）、`start()` 包装文件扫描与占位控制

### 事件系统（Event）

模块：`biliup.engine.event`

- 类：`EventManager(context: dict=None, pool: dict=None)`：线程式事件总线，支持阻塞执行池与异步派发。
  - `register(type_, block=False)`: 装饰器，将函数注册为事件处理器；返回值可为单个或多个 `Event`
  - `send_event(Event)`: 发送事件
  - `server()`: 将类实例化并把其方法中注册的事件处理器绑定到当前管理器
  - `stop()`: 停止事件循环并关闭线程池

- 类：`Event(type_: str, args: tuple=(), dict: dict={})`

- 示例：注册与触发自定义事件
```python
from biliup.engine.event import EventManager, Event

manager = EventManager()

MY_EVENT = 'my_event'

@manager.register(MY_EVENT)
def handle(arg1):
    print('handle', arg1)

manager.start()
manager.send_event(Event(MY_EVENT, args=('hello',)))
```

### 应用编排（app）

模块：`biliup.app`

- `create_event_manager() -> EventManager`：创建并初始化线程池、上下文键值
- `event_manager`：全局事件管理器实例
- `singleton_check(platform, name, url)`：同源任务单例检测与触发
- `PluginInfo`（通过 `@event_manager.server()` 注册）：维护 URL 与插件的映射关系与检测任务

### 处理管线（handler）

模块：`biliup.handler`，内置事件常量与处理器：

- 事件：`PRE_DOWNLOAD` → `DOWNLOAD` → `DOWNLOADED` → `UPLOAD` → `UPLOADED`
- 处理器：
  - `pre_processor(name, url)`：下载前处理，触发 `DOWNLOAD`
  - `process(name, url)`：执行下载，结束后触发 `DOWNLOADED`
  - `processed(stream_info)`：下载后处理，触发 `UPLOAD`
  - `process_upload(stream_info)`：组织文件与上传
  - `uploaded(name, live_cover_path, data)`：上传完成后的清理/后处理

流程可通过配置中的 `preprocessor/segment_processor/downloaded_processor/postprocessor` 钩子扩展。