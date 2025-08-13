+++
title = "组件：QRcode"
description = "扫码登录对话内容器"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 40
template = "docs/page.html"
[extra]
lead = "封装二维码获取与轮询登录，成功时回调返回 cookie 文件路径。"
toc = true
+++

源文件：`app/ui/QRcode.tsx`

### Props
- `onSuccess: (filename: string) => void`

### 用法
```tsx
<QRcode onSuccess={(path) => console.log('cookie file saved at', path)} />
```