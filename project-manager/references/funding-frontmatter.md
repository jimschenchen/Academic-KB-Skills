# Funding Note Frontmatter Schema

所有 funding/proposal note 的 YAML frontmatter 必须包含以下字段：

```yaml
---
title: "{Acronym} — {Full Name}"
tags:
  - proposal
  - {type-tag}            # funding | competition | future-project
  - {source-tags}         # 例: RGC, NSFC, 等
status: {status}          # submitted | preparing | planning | approved | rejected
type: {type}              # funding | competition | future-project
funding_source: "{来源全称}"
ref_no: "{编号}"          # 无则留空 ""
pi: "{PI 姓名}"
co_pi:                    # 可选
  - "{Co-PI 1}"
  - "{Co-PI 2}"
partners:                 # 可选：外部合作方（不同于 co_pi）
  - "{机构名}"
amount: "{金额含货币}"    # 例: "HK$5,000,000"
duration: "{时长描述}"    # 例: "36 months"
my_role: {role}           # implementation-lead | collaborator | contributor
deadline: "{YYYY-MM-DD}"  # 无则留空 ""
date: {YYYY-MM-DD}        # 创建日期
aliases:
  - {Acronym}             # 简称
---
```

## 字段说明

### status（必填）
控制 Fundings.base 中 `status_icon` formula 的显示：
- `submitted` → 📨 已提交
- `preparing` → ✏️ 准备中
- `planning` → 💡 规划中
- `approved` → ✅ 已获批
- `rejected` → ❌ 被拒

### type（必填）
控制 `type_label` formula：
- `funding` → 💰 Funding
- `competition` → 🏆 竞赛
- `future-project` → 🔮 未来项目

### my_role（必填）
- `implementation-lead` — 我负责主要实现
- `collaborator` — 深度参与
- `contributor` — 部分贡献

### tags 规范
- 第一个 tag 必须是 `proposal`（用于 Fundings.base 过滤）
- 第二个 tag 为类型 tag
- 后续 tag 为资助来源标签

## 命名约定

### 文件夹命名
格式：`{Acronym} — {Full Name}`
- 使用 em-dash（—）分隔
- 主 .md 文件名与文件夹名一致
- 例：`CRF2026 — Full Lifecycle Agentic LLM Serving Infrastructure/`

### aliases
- 简短易记的缩写，日常引用用
- 例：`CRF2026`、`Mazu 2026`
