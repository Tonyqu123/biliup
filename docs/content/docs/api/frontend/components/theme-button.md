+++
title = "组件：ThemeButton"
description = "切换主题：auto/light/dark"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 44
template = "docs/page.html"
[extra]
lead = "根据当前模式更新 `document.body[theme-mode]` 并持久化。"
toc = true
+++

源文件：`app/ui/ThemeButton.tsx`

### Props
- `mode: string`
- `setMode: (value: string | ((prev: string) => string)) => void`
- `systemTheme: string`

### 用法
```tsx
const [mode, setMode] = useState('auto')
<ThemeButton mode={mode} setMode={setMode} systemTheme={'light'} />
```