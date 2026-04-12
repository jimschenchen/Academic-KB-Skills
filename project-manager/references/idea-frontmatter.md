# Idea Note Frontmatter Schema

Ideas 使用较轻量的 frontmatter，重点在内容而非结构化属性。

```yaml
---
title: "{Idea Title}"
tags:
  - idea
  - {project-tag}        # 可选，关联项目的领域 tag
date: {YYYY-MM-DD}
project: "[[projects/Project-{Name}/Project-{Name}]]"   # wikilink 关联到项目
status: {status}          # seed | growing | mature | archived
---
```

## status 生命周期

- `seed` 🌱 — 刚冒出的想法，未展开
- `growing` 🌿 — 正在发展中，有初步分析
- `mature` 🌳 — 可以落地执行，考虑创建 project
- `archived` 🗑️ — 已放弃或已合并到其他想法

## 文件位置规则

- **已分类**：`ideas/{ProjectShortName}/{Idea Title}.md`
  - 例：`ideas/FGOALS-Agent/Agent Self-Reflection for Parameter Tuning.md`
- **未分类**：`ideas/unsorted/{Idea Title}.md`
- **模板**：`ideas/templates/Idea Template.md`

## 与项目的关联

- frontmatter 的 `project` 字段用 wikilink 指向项目
- Ideas MOC (`ideas/Ideas MOC.md`) 按项目和状态双维度索引
- Idea 成熟后（status → mature），可能升级为新 project
