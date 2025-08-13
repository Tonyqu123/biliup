+++
title = "前端 Hooks"
description = "SWR 数据获取与派生工具"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 37
template = "docs/page.html"
[extra]
lead = "包含主播列表、账号列表、分区树等 Hook。"
toc = true
+++

源文件：`app/lib/use-streamers.ts`

### useStreamers()
- 返回 `{ isLoading, streamers }`，数据源 `/v1/streamers`
```tsx
const { isLoading, streamers } = useStreamers()
```

### useBiliUsers()
- 返回 `{ isLoading, isError, biliUsers }`
- 会为每个用户拉取头像 `face`（失效时降级到默认头像）
```tsx
const { biliUsers } = useBiliUsers()
```

### useTypeTree()
- 返回 `{ isLoading, isError, typeTree }`
- 数据源 `/bili/archive/pre`，映射为选择器可用的 `{ label, value, children }`
```tsx
const { typeTree } = useTypeTree()
```