+++
title = "组件：UserList"
description = "账号管理侧边栏"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 45
template = "docs/page.html"
[extra]
lead = "列出、添加与删除账号；支持 Cookie 文件与扫码登录两种方式。"
toc = true
+++

源文件：`app/ui/UserList.tsx`

### Props
- `visible?: boolean`
- `onCancel?: (e) => void`
- `children?: React.ReactNode`

### 功能
- 列表数据来自 `useBiliUsers()`（头像自动代理下载）
- 新增用户：
  - Cookie 文件路径校验并保存到 `/v1/users`
  - 或扫码登录，写入本地 cookie 文件并保存
- 删除用户：调用 `/v1/users/{id}`

### 用法
```tsx
<UserList visible={visible} onCancel={() => setVisible(false)} />
```