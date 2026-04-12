# Changelog

All notable changes to the project-manager skill will be documented in this file.

## [0.2.0] - 2026-04-12

### Added
- **Procedure 7: Capture Idea** — 灵感速记：用户随口说想法，自动识别关联 project，创建轻量 idea note 并更新 Ideas MOC
- **idea-quick-template.md** — 轻量 idea 模板（只含 frontmatter + 核心想法 + 关联）
- Project 匹配关键词参考表，辅助自动归类
- 连续捕捉模式：支持用户连续抛出多个想法
- 更新 skill description 以覆盖灵感捕捉触发词

## [0.1.0] - 2026-04-12

### Added
- **Procedure 0: Create Project** — 从模板创建新项目，自动建立文件夹、frontmatter、更新 Ideas MOC 和 Task Kanban
- **Procedure 1: Create Funding** — 创建新 funding/proposal note，含 Tasks 表格和 info callout
- **Procedure 2: Link** — 支持 project ↔ funding / idea / paper / project 四种双向关联
- **Procedure 3: Update Status** — 状态变更 + 联动检查（下游依赖提醒、未完成任务提醒）
- **Procedure 4: Report** — 支持单项目报告、单 funding 报告、全局概览三种模式
- **Procedure 5: Sync Mapping** — 扫描所有 frontmatter 重新生成 Funding-Project Mapping.md
- **Procedure 6: Lint** — 六项健康检查（frontmatter 完整性、命名规范、关联一致性、Mapping 同步、任务状态、孤立实体）
- **Reference 文件**：project-frontmatter.md, project-note-template.md, funding-frontmatter.md, funding-note-template.md, idea-frontmatter.md
- README.md 文档

### Schema
- Project frontmatter schema：基于现有 9 个项目的实际格式提取
- Funding frontmatter schema：基于现有 7 个 funding 的实际格式提取
- Idea frontmatter schema：基于 Ideas MOC 和 Idea Template 提取
- .base formula 对应关系已文档化（status_icon, type_label, funding_count 等）
