# Funding Note Template

以下是创建新 funding/proposal note 时的标准结构：

```markdown
---
title: "{Acronym} — {Full Name}"
tags:
  - proposal
  - {type}
  - {source-tags}
status: {status}
type: {type}
funding_source: "{来源全称}"
ref_no: "{编号}"
pi: "{PI}"
co_pi:
  - "{Co-PI}"
partners:
  - "{合作方}"
amount: "{金额}"
duration: "{时长}"
my_role: {role}
deadline: "{YYYY-MM-DD}"
date: {YYYY-MM-DD}
aliases:
  - {Acronym}
---

# {Acronym} — {Full Name}

{一句话中文说明}

> [!info] 基本信息
> - **Ref No**: {编号}
> - **类型**: {资助来源全称}
> - **PI**: {PI} ({所属机构})
> - **Co-PI**: {列表}
> - **资助额**: {金额} / {时长}
> - **状态**: {中文状态}

---

## Tasks 与关联项目

| Task | 描述 | 我的角色 | 关联项目 |
|------|------|--------|--------|
| Task 1 | {描述} | 负责实现 | [[projects/Project-{Name}/Project-{Name}\|{Alias}]] |
| Task 2 | {描述} | 关联 | [[projects/Project-{Name}/Project-{Name}\|{Alias}]] |

---

## 原始文件

- [[{filename}.pdf]]
```

## 规则

1. **文件位置**：`fundings/{Acronym} — {Full Name}/{Acronym} — {Full Name}.md`
2. **PDF 文件**：原始 proposal PDF 放在同一文件夹下
3. **Tasks 表格**：每行一个 task/thrust，关联到具体 project
4. **`我的角色` 列**：标注 "负责实现" / "关联" / "部分贡献"
5. **info callout**：使用 Obsidian callout 语法展示关键元信息
