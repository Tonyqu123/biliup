+++
title = "组件：TemplateModal"
description = "新建/编辑直播间配置的弹窗"
date = 2025-08-01T08:00:00+00:00
updated = 2025-08-01T08:00:00+00:00
weight = 41
template = "docs/page.html"
[extra]
lead = "包含基本字段、后处理（postprocessor）与更多设置。"
toc = true
+++

源文件：`app/ui/TemplateModal.tsx`

### Props
- `visible?: boolean`
- `entity?: LiveStreamerEntity`
- `children?: React.ReactNode`
- `onOk: (values: any) => Promise<void>`

### 用法
```tsx
<TemplateModal onOk={async (values) => {/* 保存 */}}>
  <Button>新建直播间</Button>
</TemplateModal>
```