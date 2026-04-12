# Project Manager Skill

学术项目全生命周期管理 skill，专为 Obsidian vault 中的研究项目、Funding 提案和灵感管理而设计。

## 功能概览

| Procedure | 名称 | 功能 |
|-----------|------|------|
| 0 | Create Project | 从模板创建新项目，自动建立文件夹、frontmatter、关联 |
| 1 | Create Funding | 创建新 funding/proposal note，包含 Tasks 表格 |
| 2 | Link | 关联 project ↔ funding / idea / paper / project，双向更新 |
| 3 | Update Status | 更新状态 + 联动检查（下游依赖、未完成任务） |
| 4 | Report | 生成单项目/单 funding/全局进度报告 |
| 5 | Sync Mapping | 扫描所有 frontmatter，重新生成 Funding-Project Mapping |
| 6 | Lint | 结构健康检查（schema、命名、关联一致性、孤立实体） |
| 7 | Capture Idea | 灵感速记：自动识别 project，轻量归档，更新 Ideas MOC |

## 管理的实体

### Projects
- 位置：`projects/Project-{Name}/Project-{Name}.md`
- 数据库视图：`projects/Projects.base`
- 状态：`in-progress` / `planning` / `completed` / `paused`
- 类型：`agent-system` / `data-infrastructure` / `benchmark` / `training` / `modeling` / `infrastructure`

### Fundings
- 位置：`fundings/{Acronym} — {Full Name}/{Acronym} — {Full Name}.md`
- 数据库视图：`fundings/Fundings.base`
- 状态：`submitted` / `preparing` / `planning` / `approved` / `rejected`
- 类型：`funding` / `competition` / `future-project`

### Ideas
- 位置：`ideas/{ProjectShortName}/{Title}.md` 或 `ideas/unsorted/{Title}.md`
- 索引：`ideas/Ideas MOC.md`
- 生命周期：`seed` → `growing` → `mature` → `archived`

## 关键文件

| 文件 | 作用 |
|------|------|
| `Funding-Project Mapping.md` | 全局 funding ↔ project 映射索引 |
| `Task Dashboard.md` | obsidian-tasks 查询面板 |
| `Task Kanban.md` | 项目看板 |
| `Ideas MOC.md` | 灵感按项目 + 状态双维度索引 |
| `Research Program Overview.md` | 个人研究路线总览 |

## Reference 文件

```
references/
├── project-frontmatter.md    # Project YAML schema
├── project-note-template.md  # Project note 模板
├── funding-frontmatter.md    # Funding YAML schema
├── funding-note-template.md  # Funding note 模板
├── idea-frontmatter.md       # Idea schema + 生命周期
└── idea-quick-template.md    # 灵感速记轻量模板
```

## 与其他 Skill 的协作

- **academic-kb**：paper 的创建和管理由 academic-kb 负责，本 skill 只在 project note 中添加 wikilink 引用
- **brainstorming**：深入某个 project 的技术方案时，建议使用 brainstorming skill
- **obsidian-bases**：.base 文件的修改由 obsidian-bases skill 负责

## 使用示例

```
> 帮我创建一个新项目 Project-WeatherAgent
> 把 CRF2026 关联到 Project-WeatherAgent
> 更新 DisasterDB 状态为 completed
> 给我一个全局进度报告
> 同步一下 Funding-Project Mapping
> 检查 vault 结构健康状况
> 我觉得 DisasterDB 可以加一个自动数据质量检测的模块
> 突然想到，harness 的 skill 注册可以做成去中心化的
```
