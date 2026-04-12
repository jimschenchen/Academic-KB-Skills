---
name: project-manager
description: >-
  Academic project lifecycle management for Obsidian vaults — create projects and fundings,
  link them together, capture ideas into projects, track deliverables, sync the Funding-Project
  Mapping, generate progress reports, and lint for structural health. Use this skill whenever
  the user wants to: create a new project or funding proposal, link projects to fundings or
  ideas, capture a thought or idea related to a project, update project status, check project
  progress, generate a report, sync mappings, or lint the vault structure. Also trigger when
  the user says things like 'new project', 'add funding', 'link this to', 'project status',
  'progress report', 'sync mapping', 'check vault health', or when the user casually mentions
  an idea, thought, or inspiration related to their research projects — e.g. '我觉得可以...',
  '突然想到...', 'what if we...', '有个想法', 'idea for', or mentions managing projects,
  proposals, deliverables, or tasks.
---

# Project Manager Skill

管理 Obsidian vault 中的学术项目全生命周期：项目创建、Funding 关联、状态跟踪、进度报告、结构同步与健康检查。

## Vault 结构概览

```
vault-root/
├── projects/
│   ├── Projects.base                    # 项目数据库视图
│   └── Project-{Name}/
│       ├── Project-{Name}.md            # 主项目 note
│       └── {支撑文档}.md               # 执行计划、实现指南等
├── fundings/
│   ├── Fundings.base                    # Funding 数据库视图
│   ├── Funding-Project Mapping.md       # 核心映射索引
│   └── {Acronym} — {Full Name}/
│       ├── {Acronym} — {Full Name}.md   # 主 funding note
│       └── {原始文件}.pdf              # 提交的 PDF 等
├── ideas/
│   ├── Ideas MOC.md                     # 灵感索引
│   ├── templates/Idea Template.md
│   ├── {ProjectShortName}/             # 按项目分类
│   └── unsorted/                       # 未分类
├── Task Dashboard.md                    # obsidian-tasks 查询面板
└── Task Kanban.md                       # Kanban 看板
```

## 参考文件

以下 reference 文件包含详细的 schema 定义和模板。**在执行具体 Procedure 前，先 Read 对应的 reference 文件。**

| 文件 | 用途 | 何时读取 |
|------|------|---------|
| `references/project-frontmatter.md` | Project YAML schema + 字段说明 | Procedure 0, 3, 6 |
| `references/project-note-template.md` | Project note 完整模板 | Procedure 0 |
| `references/funding-frontmatter.md` | Funding YAML schema + 字段说明 | Procedure 1, 3, 6 |
| `references/funding-note-template.md` | Funding note 完整模板 | Procedure 1 |
| `references/idea-frontmatter.md` | Idea schema + 生命周期 | Procedure 2, 6, 7 |
| `references/idea-quick-template.md` | 灵感速记轻量模板 | Procedure 7 |

---

## Procedure 0: Create Project — 创建新项目

**触发**：用户想创建新项目、新 research 方向、或将 idea 升级为项目。

### 步骤

1. **收集信息**（向用户确认）：
   - 项目名称（英文 CamelCase + 中文副标题）
   - `project_type`（从 6 种类型中选择）
   - 初始 `status`（通常 `planning`）
   - 关联的 funding（可选，可后续用 Procedure 2 添加）
   - 简要描述（1-2 段）
   - 初始目标列表

2. **Read reference 文件**：
   - `references/project-frontmatter.md` — 确认 schema
   - `references/project-note-template.md` — 获取模板

3. **创建文件结构**：
   - 创建文件夹：`projects/Project-{Name}/`
   - 创建主 note：`projects/Project-{Name}/Project-{Name}.md`
   - 按模板填充 frontmatter 和所有标准 section
   - `date` 使用当天日期

4. **更新关联**（如有 funding）：
   - 在 project note 的 `fundings` 数组中添加 wikilink
   - 在对应 funding note 的 Tasks 表格中添加行
   - 运行 Procedure 5（Sync Mapping）更新 Funding-Project Mapping

5. **更新 Ideas MOC**：
   - 在 `ideas/Ideas MOC.md` 的「按项目分类」下添加该项目的 section
   - 如果是从 idea 升级的，更新原 idea 的 status 为 `archived`

6. **更新 Task Kanban**：
   - 在 `Task Kanban.md` 中添加新列（项目名称）

7. **确认输出**：向用户报告创建的文件和更新的关联。

### 命名规范

- 文件夹和文件名：`Project-{CamelCaseName}`
- title 格式：`"Project: {Name} — {中文副标题}"`
- aliases：至少包含简称（不含 `Project-` 前缀）

---

## Procedure 1: Create Funding — 创建新 Funding/Proposal

**触发**：用户想记录新的资助申请、竞赛或未来项目构想。

### 步骤

1. **收集信息**（向用户确认）：
   - Acronym + Full Name（中英文）
   - `type`（funding / competition / future-project）
   - `funding_source`、`pi`、`co_pi`
   - `amount`、`duration`
   - `my_role`（implementation-lead / collaborator / contributor）
   - `deadline`（如有）
   - 关联的 projects（可选）

2. **Read reference 文件**：
   - `references/funding-frontmatter.md`
   - `references/funding-note-template.md`

3. **创建文件结构**：
   - 创建文件夹：`fundings/{Acronym} — {Full Name}/`
   - 创建主 note：`fundings/{Acronym} — {Full Name}/{Acronym} — {Full Name}.md`
   - 按模板填充所有 section
   - 如有关联 projects，填写 Tasks 表格

4. **更新关联 projects**（如有）：
   - 在每个关联 project 的 frontmatter `fundings` 数组中添加 wikilink
   - 在 project note 的「Funding 任务分解」section 添加对应 h3 小节

5. **运行 Procedure 5**（Sync Mapping）更新全局映射。

6. **确认输出**。

### 命名规范

- 文件夹名：`{Acronym} — {Full Name}`（em-dash 分隔）
- aliases：缩写形式，如 `CRF2026`

---

## Procedure 2: Link — 关联实体

**触发**：用户想把 project 关联到 funding、idea、paper 或其他 project。

### 支持的关联类型

| 关联方向 | 操作 |
|---------|------|
| Project ↔ Funding | 双向更新 frontmatter + Mapping |
| Project ↔ Idea | 更新 idea 的 `project` 字段 + Ideas MOC |
| Project ↔ Paper | 在 project note 的 Related Notes 添加 wikilink |
| Project ↔ Project | 更新双方的「依赖关系」section |

### 步骤

1. **确认关联**：识别或询问用户要关联的两个实体。

2. **执行关联**（根据类型）：

   **Project ↔ Funding**：
   - 在 project frontmatter 的 `fundings` 数组中添加 funding wikilink
   - 在 project note 的「Funding 任务分解」添加 h3 小节（向用户确认 task 描述）
   - 在 funding note 的 Tasks 表格中添加行
   - 运行 Procedure 5 同步 Mapping

   **Project ↔ Idea**：
   - 更新 idea note frontmatter 的 `project` 字段
   - 如果 idea 在 `ideas/unsorted/`，移动到 `ideas/{ProjectShortName}/`
   - 更新 Ideas MOC 中的分类

   **Project ↔ Paper**：
   - 在 project note 的「Related Notes」section 添加 `[[papers/{Title}/{Title}]]`
   - 如果 paper note 存在，在其 frontmatter tags 中添加项目相关 tag

   **Project ↔ Project**：
   - 确认依赖方向（上游/下游/相关）
   - 在双方的「依赖关系」section 中添加 wikilink 和说明

3. **确认输出**：列出所有修改的文件。

---

## Procedure 3: Update Status — 更新状态

**触发**：用户想改变项目或 funding 的状态。

### 步骤

1. **确认变更**：
   - 目标实体（project 或 funding）
   - 新 status 值
   - 变更原因（可选，记录在 note 中）

2. **更新 frontmatter**：
   - 修改 `status` 字段
   - 如果是 project，同步更新 tags 中的状态 tag（`active` ↔ `planning` ↔ `completed` ↔ `paused`）

3. **联动检查**（仅当状态变为 `completed` 或 `paused`）：
   - 检查该 project 的下游依赖项目，提醒用户
   - 检查未完成的 tasks（`- [ ]`），提醒用户
   - 如果是 funding 变为 `rejected`，提醒用户检查关联 projects 是否需要调整

4. **记录变更**：在 note 的「当前进度」或顶部添加一行变更记录：
   ```markdown
   > [!note] {YYYY-MM-DD} 状态变更：{旧状态} → {新状态}
   > {原因}
   ```

5. **确认输出**。

---

## Procedure 4: Report — 生成进度报告

**触发**：用户想了解项目进度、funding 覆盖情况或全局概览。

### 报告类型

#### 4a: 单项目报告

1. Read 目标 project note。
2. 统计：
   - 总 tasks 数、已完成数、完成率
   - 关联 funding 数量
   - 交付物状态分布
3. 输出格式：

```markdown
## 📊 Project-{Name} 进度报告 ({date})

**状态**: {status_icon}
**类型**: {type_label}
**关联 Funding**: {count} 个

### 任务进度
- 总任务: {total} | 已完成: {done} | 进行中: {total - done}
- 完成率: {percentage}%

### 交付物状态
| 交付物 | 来源 | 状态 |
|--------|------|------|
| ... | ... | ... |

### 未完成的关键任务
- [ ] ...

### 依赖状态
- 上游: {列表及其状态}
- 下游: {列表及其状态}
```

#### 4b: Funding 报告

1. Read 目标 funding note。
2. 统计关联 projects 的进度。
3. 列出 tasks 和对应 project 的完成情况。

#### 4c: 全局概览

1. Glob `projects/Project-*/Project-*.md` 获取所有 project notes。
2. Glob `fundings/*/[!F]*.md` 获取所有 funding notes（排除 Funding-Project Mapping）。
3. 汇总：
   - 各状态的项目数量
   - 各类型的项目分布
   - 有/无 funding 的项目
   - 临近 deadline 的 funding
   - 整体 task 完成率

---

## Procedure 5: Sync Mapping — 同步 Funding-Project 映射

**触发**：用户要求同步映射，或在 Procedure 0/1/2 中自动触发。

### 步骤

1. **扫描所有 project notes**：
   - Glob `projects/Project-*/Project-*.md`
   - 从每个 note 提取 frontmatter 的 `fundings` 数组
   - 构建 project → fundings 反向索引

2. **扫描所有 funding notes**：
   - Glob `fundings/*/[!F]*.md`
   - 从每个 note 提取 frontmatter（title, aliases, pi, amount 等）
   - 读取 Tasks 表格获取 project 关联和 task 描述

3. **重新生成 `Funding-Project Mapping.md`**：
   - 保持现有结构不变
   - **Fundings section**：每个 funding 一个 h3，包含 blockquote 元信息 + project 列表
   - **反向索引 section**：Project → Fundings 的表格
   - 格式参考现有文件：
     ```markdown
     ### {Funding Title}

     > {funding_source} | PI: {pi} | {amount}

     - [[projects/Project-{Name}/Project-{Name}|{Alias}]] — {Task描述}
     ```

4. **差异检查**：
   - 对比更新前后的 Mapping 内容
   - 报告新增/删除/修改的映射关系

5. **确认输出**。

### 重要规则

- Mapping 的 Fundings section 按 funding note 的 aliases 字母排序
- 反向索引按 project 名称排序
- wikilink 格式：`[[projects/Project-{Name}/Project-{Name}|{ShortAlias}]]`
- funding 的 blockquote 包含：来源 | PI | 金额（如有）

---

## Procedure 6: Lint — 结构健康检查

**触发**：用户想检查 vault 的项目管理结构是否健康。

### 检查项

#### 6a: Frontmatter 完整性
- 所有 project notes 是否包含必填字段（tags 含 `project`、status、project_name、project_type、date）
- 所有 funding notes 是否包含必填字段（tags 含 `proposal`、status、type、my_role、date）
- idea notes 是否包含 `tags: [idea]`、status、date

#### 6b: 命名规范
- Project 文件夹是否为 `Project-{CamelCase}` 格式
- Project .md 文件名是否与文件夹名一致
- **特殊项目**：某些 project 文件夹（如 `Project-PQE`、`Project-Awesome-AI-for-Earth-Science`）可能不含标准 project note，而是包含其他类型文档（考试材料、GitHub repo docs）。这些应标记为 ⚠️ 警告而非 ❌ 错误
- Funding 文件夹是否使用 em-dash（—）分隔
- Funding .md 文件名是否与文件夹名一致
- **排除非主文件**：funding 文件夹中的会议纪要、PDF 等辅助文件不做 schema 检查，只检查与文件夹同名的 .md 主文件

#### 6c: 关联一致性
- Project frontmatter 的 `fundings` 中每个 wikilink 是否指向存在的 funding note
- Funding note Tasks 表格中的 project wikilink 是否指向存在的 project note
- **双向检查**：如果 project A 声称关联 funding X，funding X 的 Tasks 表格是否也包含 project A
- Ideas MOC 中列出的 idea 文件是否存在

#### 6d: Mapping 同步状态
- Funding-Project Mapping.md 的内容是否与各 note 的 frontmatter 一致
- 如不一致，列出差异并建议运行 Procedure 5

#### 6e: 任务状态
- 是否有 `status: completed` 的项目仍有未完成的 tasks
- 是否有 `status: in-progress` 的项目没有任何 tasks
- Task Kanban 中的列是否覆盖所有活跃项目

#### 6f: 孤立实体
- 没有关联任何 funding 的 project（可能是正常的，但需提醒）
- 没有关联任何 project 的 funding（应该报告）
- `ideas/unsorted/` 中的 ideas 是否需要分类

### 输出格式

```markdown
## 🔍 Vault 健康检查 ({date})

### ✅ 通过 ({count})
- ...

### ⚠️ 警告 ({count})
- ...

### ❌ 错误 ({count})
- ...

### 📋 建议操作
1. ...
2. ...
```

分为三个级别：
- **✅ 通过**：符合规范
- **⚠️ 警告**：不影响功能但建议修复（如孤立项目、缺少 aliases）
- **❌ 错误**：需要修复（断链、schema 不完整、双向不一致）

---

## Procedure 7: Capture Idea — 灵感速记

**触发**：用户随口提到一个想法、灵感、直觉，不论是否明确提到项目名。这是最轻量的操作——用户说想法，skill 自动归档。

### 核心原则

- **零摩擦**：不问多余问题，不套完整模板，速度优先
- **智能归类**：根据想法内容自动识别关联 project
- **最小记录**：只记核心想法（1-3 句），日后可展开

### 步骤

1. **解析用户输入**：
   - 提取核心想法（用户说了什么）
   - 推断关联 project：
     - 如果用户明确提到项目名 → 直接使用
     - 如果用户提到领域关键词（如 "灾害数据库"、"早期预警"、"harness"）→ 匹配最相关 project
     - 如果无法判断 → 放入 `ideas/unsorted/`，不打断用户去问

2. **生成 idea title**：
   - 从用户的话中提取简洁英文标题（3-8 个词）
   - 标题应反映核心想法，不是项目名
   - 例："Agent Self-Reflection for Parameter Tuning"

3. **创建轻量 idea note**：
   - Read `references/idea-quick-template.md` 获取轻量模板
   - 文件位置：
     - 已归类：`ideas/{ProjectShortName}/{Title}.md`
     - 未归类：`ideas/unsorted/{Title}.md`
   - 用轻量模板，只填核心内容

4. **更新 Ideas MOC**：
   - 在 `ideas/Ideas MOC.md` 对应项目 section 下添加 wikilink
   - 在「最近添加」section 顶部添加一行（含日期）
   - 如果放入 unsorted，添加到「未分类」section

5. **简短确认**：
   - 一句话告诉用户：想法已记录到 `{路径}`，归类到 `{ProjectName}`
   - 不要长篇大论解释做了什么

### Project 匹配关键词参考

在无法确定时，参考以下关键词映射：

| 关键词 | Project |
|--------|---------|
| 灾害、灾害数据库、disaster、hazard、data pipeline | Project-DisasterDB |
| FGOALS、参数调优、climate model、物理模型 | Project-FGOALS-Agent |
| 早期预警、early warning、决策支持、台风 | Project-EarlyWarningAgent |
| harness、skill framework、tool、agent infra | Project-AtmosAgentHarness |
| RL、强化学习、reward、multi-agent | Project-RL-Climate-Agents |
| benchmark、评测、evaluation、S2S | Project-S2SServiceBench |
| earth model、世界模型、多圈层、耦合 | Project-EarthWorldModel |
| awesome、survey、开源 | Project-Awesome-AI-for-Earth-Science |

> **注意**：这张表只是辅助。优先根据用户的实际语义判断，不要死板匹配关键词。如果想法跨多个项目，选最核心的那个，或放 unsorted。

### 连续捕捉模式

如果用户在短时间内连续抛出多个想法：
- 逐个创建 note，不要合并
- 每个想法独立归类
- 批量确认：最后一起报告所有记录的想法

---

## 通用规则

### Wikilink 格式
- Project 引用：`[[projects/Project-{Name}/Project-{Name}|{ShortAlias}]]`
- Funding 引用：`[[{FolderName}|{Acronym}]]`
- Ideas 引用：`[[ideas/{Folder}/{Title}]]`
- Papers 引用：`[[papers/{Title}/{Title}]]`

### 日期格式
- 所有日期使用 `YYYY-MM-DD`（ISO 8601）
- 用户说"周四"时，转换为绝对日期

### 语言规范
- Frontmatter 字段名：全英文
- 内容正文：中英混合（与 vault 现有风格一致）
- Section 标题：中文（带括号英文），如 `## 项目概述`、`## 目标 (Goals)`

### 文件操作安全
- 修改文件前，先 Read 确认当前内容
- 编辑 frontmatter 时使用精确的 string replacement，避免误改正文
- 创建新文件时检查是否已存在同名文件/文件夹
- 涉及多个文件修改时，逐一确认后再执行

### 与其他 Skill 的协作
- 当用户要求关联 paper 时，paper note 应由 **academic-kb** skill 管理
- 本 skill 只在 project note 的 Related Notes 中添加 wikilink
- 如果用户想深入某个 project 的技术方案，建议使用 **brainstorming** skill
