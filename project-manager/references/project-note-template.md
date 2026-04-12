# Project Note Template

以下是创建新项目 note 时的标准结构：

```markdown
---
title: "Project: {Name} — {中文副标题}"
tags:
  - project
  - {status-tag}
  - {domain-tags}
status: {status}
date: {YYYY-MM-DD}
project_name: Project-{Name}
project_type: {type}
fundings:
  - "[[{FundingFolder}|{Alias}]]"
aliases:
  - {ShortName}
---

# Project: {Name} — {中文副标题}

## 项目概述

{1-2 段描述项目目标、核心能力和预期影响。中英混合。}

---

## 目标 (Goals)

- {目标1}
- {目标2}
- ...

---

## Funding 任务分解

### 来自 [[{FundingName}|{Alias}]]

*{Task/Thrust 描述}*

{简要说明该 funding 下本项目需要完成的工作。}

**任务：**
- {任务1}
- {任务2}

**交付物：**
- {交付物1}
- {交付物2}

---

## 交付物汇总

| 交付物 | 来源 Funding | 状态 |
|--------|------------|------|
| {名称} | {Funding Alias} | Planned / In Progress / Done |

---

## 当前进度

- [ ] {待办事项1}
- [ ] {待办事项2}
- [ ] ...

---

## 依赖关系

**上游依赖：**
- [[projects/Project-{Name}/Project-{Name}|{Name}]] — {说明}

**本项目被下游项目依赖：**
- [[projects/Project-{Name}/Project-{Name}|{Name}]] — {说明}

---

## Related Notes

- [[{相关 note 的 wikilink}]] — {简要说明}
```

## 规则

1. **文件位置**：`projects/Project-{Name}/Project-{Name}.md`
2. **文件夹命名**：`Project-{CamelCaseName}`，与 `project_name` 字段一致
3. **Funding 任务分解**：每个关联的 funding 单独一个 h3 小节
4. **交付物汇总**：汇总所有 funding 来源的交付物
5. **当前进度**：使用 `- [ ]` / `- [x]` 语法，obsidian-tasks 插件会自动索引
6. **依赖关系**：明确标注上游/下游，便于影响分析
