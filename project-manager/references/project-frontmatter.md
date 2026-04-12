# Project Frontmatter Schema

所有 project note 的 YAML frontmatter 必须包含以下字段：

```yaml
---
title: "Project: {Name} — {中文副标题}"
tags:
  - project
  - {status-tag}          # active | planning | completed | paused
  - {domain-tags}         # 例: agent, disaster-database, benchmark
status: {status}          # in-progress | planning | completed | paused
date: {YYYY-MM-DD}       # 创建日期
project_name: Project-{CamelCaseName}
project_type: {type}      # agent-system | data-infrastructure | benchmark | training | modeling | infrastructure
fundings:                 # 关联 funding，wikilink 数组
  - "[[{FundingFolder}|{Alias}]]"
aliases:
  - {ShortName}           # 简称，用于日常引用
related_projects:         # 可选：关联项目
  - "[[projects/Project-{Name}/Project-{Name}]]"
---
```

## 字段说明

### status（必填）
控制 Projects.base 中 `status_icon` formula 的显示：
- `in-progress` → 🚀 进行中
- `planning` → 💡 规划中
- `completed` → ✅ 已完成
- `paused` → ⏸️ 暂停

### project_type（必填）
控制 Projects.base 中 `type_label` formula 的显示：
- `agent-system` → 🤖 Agent System
- `data-infrastructure` → 🗄️ Data Infra
- `benchmark` → 📊 Benchmark
- `training` → 🎯 Training
- `modeling` → 🌍 Modeling
- `infrastructure` → 🔧 Infrastructure

### fundings（可选，数组）
wikilink 格式指向 funding note，使用 `|Alias` 显示简称：
```yaml
fundings:
  - "[[CRF2026 — Full Lifecycle Agentic LLM Serving Infrastructure|CRF2026]]"
```

### tags 规范
- 第一个 tag 必须是 `project`（用于 .base 过滤）
- 第二个 tag 建议为状态 tag（`active`/`planning` 等）
- 后续 tag 为领域标签，自由添加
