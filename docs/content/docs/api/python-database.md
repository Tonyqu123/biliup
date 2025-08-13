+++
title = "数据库模型与工具"
description = "ORM 模型字段、数据库初始化与常用操作"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 34
template = "docs/page.html"
[extra]
lead = "使用 SQLite 持久化下载信息、模板与空间配置。"
toc = true
+++

### ORM 模型（`biliup.database.models`）

- `StreamerInfo`（下载信息）
  - `id: int`
  - `name: str`（主播名/索引）
  - `url: str`（源地址）
  - `title: str`（直播标题）
  - `date: datetime`（开播时间）
  - `live_cover_path: str`（封面路径）
  - `filelist: List[FileList]`

- `FileList`（会话内的视频文件名）
  - `id: int`
  - `file: str`
  - `streamer_info_id: int`（外键，级联删除）

- `Configuration`（键值配置表）
  - `id: int`
  - `key: str`
  - `value: TEXT`

- `UploadStreamers`（投稿模板）
  - `id: int`
  - `template_name: str`
  - `title/tid/copyright/copyright_source/cover_path/description/dynamic/dtime`
  - `dolby/hires/open_elec/no_reprint`
  - `uploader: str`
  - `user_cookie: str`
  - `tags: JSON`、`credits: JSON`
  - `up_selection_reply/up_close_reply/up_close_danmu`
  - `extra_fields: str`

- `LiveStreamers`（直播间配置）
  - `id: int`
  - `url: str`（唯一）
  - `remark: str`（等同 streamer 名）
  - `filename_prefix: str`
  - `time_range: str`
  - `upload_streamers_id: int | None`
  - `format: str`
  - `override/preprocessor/segment_processor/downloaded_processor/postprocessor/opt_args: JSON`

### 数据库初始化（`biliup.database.db.init`）

- `init(no_http: bool, from_config: bool) -> bool`
  - 创建表、触发 Alembic 自动迁移；在某些场景下备份旧库

### 常用查询与写入

- `get_stream_info(db, name) -> dict`: 最近一次下载信息（按 id 降序）
- `get_stream_info_by_filename(db, filename) -> dict`
- `add_stream_info(db, name, url, date) -> int`
- `delete_stream_info(db, name) -> int`
- `delete_stream_info_by_date(db, name, date) -> int`
- `update_cover_path(db, id, live_cover_path)`
- `update_room_title(db, id, title)`
- `update_file_list(db, id, file_name) -> int`
- `get_file_list(db, id) -> List[str]`

### 会话工厂

- `SessionLocal = sessionmaker(bind=engine, autocommit=False)`

### 辅助函数

- `struct_time_to_datetime(date)`、`datetime_to_struct_time(date)`：时间互转

### 示例

```python
from biliup.database.db import SessionLocal, add_stream_info, get_stream_info
import time

with SessionLocal() as db:
    row_id = add_stream_info(db, 'my-streamer', 'https://example.com/live', time.localtime())
    info = get_stream_info(db, 'my-streamer')
    print(row_id, info)
```