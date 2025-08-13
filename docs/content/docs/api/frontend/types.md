+++
title = "前端类型"
description = "接口类型：直播间、投稿模板、用户等"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 39
template = "docs/page.html"
[extra]
lead = "源自 `app/lib/api-streamer.ts` 的类型定义。"
toc = true
+++

### StudioEntity（投稿模板）
- `id: number`
- `template_name: string`
- `user_cookie: string`
- `copyright: number`
- `copyright_source: string`
- `tid: number`
- `cover_path: string`
- `title/description/dynamic/tags: string | string[]`
- `dtime: number`
- `mission_id?: number`
- `dolby/hires/no_reprint/open_elec: number`
- `up_selection_reply/up_close_reply/up_close_danmu: number`
- `credits: { username: string; uid: number; }[]`
- `uploader: string`
- `extra_fields?: string`

### LiveStreamerEntity（直播间）
- `id: number`
- `url: string`
- `remark: string`
- `filename: string`
- `split_time?: number`
- `split_size?: number`
- `upload_id?: number`
- `status?: string | React.ReactNode`
- `format?: string`
- `time_range?: string`
- `preprocessor/segment_processor/downloaded_processor/postprocessor?: ...`
- `opt_args?: string[]`
- `override?: Record<string, any>`

### BiliType
- `id: number`、`name: string`、`desc: string`、`children: BiliType[]`

### User
- `id: number`、`name: string`、`value: string`、`platform: string`

### FileList
- `key: number`、`name: string`、`updateTime: number`、`size: number`