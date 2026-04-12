# Skills Dev — 自研 Skill 开发仓库

这里存放我自主开发和维护的 Claude Skills 源码。每个 skill 是一个独立文件夹，包含 SKILL.md（核心指令）、references/（模板和参考资料）以及文档。

开发完成后，将 skill 文件夹复制到 `.claude/skills/` 即可部署使用。

---

## 三个 Skill 概览

### 1. academic-kb — 学术知识库管理

管理 Obsidian vault 中的论文、文章和主题综合笔记。

**核心能力：**
- 论文 Ingest：PDF → 12 段双语结构化笔记 + 可选的逐章深读笔记
- 文章 Ingest：URL/文本 → 5 段笔记
- Topic Compile：跨多篇论文/文章生成主题综合分析
- Cross-corpus Query：跨语料库的研究问答
- Vault Lint：检查孤立笔记、断链、缺失字段
- 操作日志：所有变更记录到 kb-log.md

**管理的数据：** 46 篇论文笔记、6 篇文章笔记、11 个主题综合、3 个研究 Query

**文件结构：**
```
academic-kb/
├── SKILL.md              # 核心指令（~600 行，6 个 Procedure）
├── README.md / README.zh.md
├── CHANGELOG.md
├── LICENSE
└── references/
    ├── paper-frontmatter.md
    ├── paper-note-template.md
    ├── section-note-template.md
    ├── article-frontmatter.md
    ├── article-note-template.md
    └── template.css
```

---

### 2. project-manager — 学术项目全生命周期管理

管理研究项目、Funding 提案、灵感捕捉，以及它们之间的关联网络。

**核心能力：**
- 创建项目 / Funding（标准化 frontmatter + 模板）
- 关联管理：project ↔ funding / idea / paper / project 四种双向链接
- 状态变更 + 联动检查（下游依赖提醒、未完成任务提醒）
- 进度报告（单项目 / 单 Funding / 全局概览）
- Funding-Project Mapping 自动同步
- Vault 结构健康检查（六维 Lint）
- 灵感速记：随口说想法，自动识别 project 并轻量归档

**管理的数据：** 9 个项目、7 个 Funding 提案、8+ 个灵感笔记，通过 .base 数据库视图和 Mapping 索引互联

**文件结构：**
```
project-manager/
├── SKILL.md              # 核心指令（~470 行，8 个 Procedure）
├── README.md
├── CHANGELOG.md
└── references/
    ├── project-frontmatter.md
    ├── project-note-template.md
    ├── funding-frontmatter.md
    ├── funding-note-template.md
    ├── idea-frontmatter.md
    └── idea-quick-template.md
```

---

### 3. english-learning — 英语间隔重复学习系统

基于 SM-2 算法的英语词汇和表达学习系统，存储在 Obsidian markdown 中。

**核心能力：**
- 添加词汇/句子/用法：自动生成中文翻译、发音、语境例句、学习笔记
- 间隔复习：SM-2 算法调度，7 种题型轮换（英译中、中译英、填空、造句、选择、纠错、情景）
- 自适应难度：根据正确率动态调整题型难度
- 进度仪表盘：学习统计、trouble words、复习日程
- 批量导入：粘贴段落自动提取生词

**管理的数据：** words/、sentences/、usage/ 三个子目录下的词条文件

**文件结构：**
```
english-learning/
├── SKILL.md              # 核心指令（~210 行，4 个 Workflow）
└── references/
    ├── sm2-algorithm.md   # SM-2 算法详细实现
    └── quiz-templates.md  # 题型模板与选择逻辑
```

---

## Skill 之间的协作关系

```
academic-kb ──论文关联──→ project-manager
    │                         │
    │ 论文中的生词             │ 灵感中的术语
    ↓                         ↓
         english-learning
```

- **academic-kb → project-manager**：论文通过 wikilink 关联到项目的 Related Notes
- **academic-kb → english-learning**：阅读论文时遇到的生词可以直接加入学习系统
- **project-manager → academic-kb**：项目笔记引用论文，但论文的管理（ingest、deep-read）由 academic-kb 负责

---

## 开发 → 部署

```bash
# 将 skill 部署到 Claude Skills 目录
cp -r skills-dev/{skill-name}/ .claude/skills/{skill-name}/
```

部署后 skill 在 Cowork / Claude Code 中自动生效。
