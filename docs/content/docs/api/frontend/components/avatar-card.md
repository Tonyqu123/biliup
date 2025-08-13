+++
title = "组件：AvatarCard"
description = "用户信息卡片，带删除确认"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 43
template = "docs/page.html"
[extra]
lead = "显示头像、昵称与标识，并提供删除确认弹层。"
toc = true
+++

源文件：`app/ui/AvatarCard/index.tsx`

### Props
- `abbr: string`：头像占位文本
- `url?: string`：头像 URL
- `label: string`：名称
- `value: string`：标识/路径
- `onRemove?: (e: any) => Promise<any> | void`

### 用法
```tsx
<AvatarCard abbr="UP" url={avatar} label="某UP" value="cookies.json" onRemove={remove} />
```