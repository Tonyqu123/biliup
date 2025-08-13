+++
title = "组件：OverrideModal"
description = "对单个直播间进行临时/精细化配置覆写"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 42
template = "docs/page.html"
[extra]
lead = "支持 JSON 文本覆写与平台特定设置（B 站、虎牙、斗鱼、YouTube 等）。"
toc = true
+++

源文件：`app/ui/OverrideModal.tsx`

### Props
- `visible?: boolean`
- `entity?: LiveStreamerEntity`
- `children?: React.ReactNode`
- `onOk: (values: any) => Promise<void>`

### 说明
- 解析 `override_text`（JSON）后合并到 `values.override`
- 自动过滤非 `LiveStreamerEntity` 字段到 `override` 中
- 根据 `entity.url` 匹配平台插件，展示平台定制选项

### 用法
```tsx
<OverrideModal entity={live} onOk={saveOverride}>
  <Button>配置覆写</Button>
</OverrideModal>
```