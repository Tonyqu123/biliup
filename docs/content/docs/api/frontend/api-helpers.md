+++
title = "前端 API 工具"
description = "基于 fetch 的请求封装"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 36
template = "docs/page.html"
[extra]
lead = "统一拼接 NEXT_PUBLIC_API_SERVER，处理异常与 JSON 解析。"
toc = true
+++

源文件：`app/lib/api-streamer.ts`

### sendRequest<T>(url, { arg })
- POST JSON，返回解析后的数据；非 2xx 抛错（包含文本或 message）
```ts
await sendRequest('/v1/users', { arg: { id: 0, name: 'foo', value: 'cookies.json', platform: 'bilibili-cookies' } })
```

### fetcher(...args)
- 通用 GET/请求封装，失败抛错，成功返回 `res.json()`；适配 SWR
```ts
const { data } = useSWR('/v1/streamers', fetcher)
```

### proxy(input, init?)
- 透传 fetch；仅拼接前缀，返回 Response
```ts
const res = await proxy('/bili/proxy?url=https://i0.hdslb.com/bfs/face/xxx.jpg')
```

### requestDelete<T>(url, { arg })
- 发送 DELETE 到 `${url}/${arg}`

### put<T>(url, { arg })
- 发送 PUT JSON 到 `${url}`