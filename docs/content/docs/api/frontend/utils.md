+++
title = "前端工具"
description = "响应式、时间与主题工具函数"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 38
template = "docs/page.html"
[extra]
lead = "常用的媒体查询注册、时间格式化与主题切换。"
toc = true
+++

源文件：`app/lib/utils/index.tsx`

### responsiveMap
- 断点映射对象，可配合 `window.matchMedia` 使用

### registerMediaQuery(media, { match, unmatch, callInInit })
- 监听媒体查询变化，返回取消监听的函数
```ts
const cancel = registerMediaQuery('(min-width: 768px)', {
  match: () => console.log('>= md'),
  unmatch: () => console.log('< md'),
})
```

### humDate(unixSeconds)
- 将秒级时间戳格式化为 `YYYY-MM-DD HH:mm:ss`

### useSystemTheme()
- 返回系统主题 `dark | light`

### useTheme(mode, systemTheme)
- 将 `mode` 应用到 `document.body[theme-mode]`，并持久化到 localStorage