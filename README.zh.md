# academic-kb: Karpathy LLM Wiki 的学术版

管理 Obsidian 学术 vault 的完整流水线：**ingest → read → compile → query → lint**。支持学术论文（PDF）和文章（博客、技术报告、白皮书、社交媒体）。

灵感来源：
- [Karpathy 的 knowledge base 工作流](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — ingest → compile → query → lint 四阶段流水线的核心思想
- [Paperwise (sjqsgg)](https://github.com/sjqsgg/Paperwise) — PDF 精读标注的双栏 HTML 设计与 5 色高亮系统

## 安装

将 `academic-kb.skill` 文件拖拽到 Claude Desktop 的对话窗口中即可安装。或者将 `academic-kb/` 文件夹复制到你的 `.claude/skills/` 目录下。

**建议配合 [Obsidian](https://obsidian.md) 使用。** 本 skill 生成的所有 paper notes 和 topic 文件都采用 Obsidian Flavored Markdown（wikilinks、callouts、frontmatter properties），并依赖 Obsidian 的 Dataview 插件实现 Paper Database 的自动查询和聚合视图。安装 Obsidian 后，直接将 vault 根目录作为 Obsidian vault 打开即可获得完整的双向链接、图谱可视化和 Dataview 仪表盘体验。

## 快速上手

### 0. Ingest — 导入一篇新论文

```
/kb ingest papers/AlphaEvolve/AlphaEvolve.pdf
```

也可以直接给论文名称、arXiv ID 或 URL，会自动下载 PDF：

```
/kb ingest 2502.13131
/kb ingest "AlphaEvolve: A coding agent for scientific discovery"
/kb ingest https://arxiv.org/abs/2502.13131
```

**做了什么：**
- 如果输入是标题、arXiv ID、DOI 或 URL，先下载 PDF（下载前会跟你确认）
- 解析 PDF，提取元数据和全文
- 生成结构化中英双语 paper note（12 个 section 0-11，每个 section 中英并列 + 完整 frontmatter）
- 如果已有 `/kb read` 产出的 HTML annotation，会自动利用它
- **自动深读：** 对于长论文/复杂论文（≥12 页、survey 类、高相关度），自动在 `sections/` 中创建逐章深读笔记，提取 insight、关键引文和跨论文关联
- 自动建议 topics 归类，更新对应 topic 文件的「相关论文与资源」
- 在正文中嵌入 5-10+ 个 wikilinks（到 topics 和其他 papers）
- 追加 kb-log 记录

**Flags:** `--deep`（强制生成 section notes）, `--no-deep`（跳过 section notes）

### 0b. Ingest Article — 导入一篇文章

```
/kb ingest article https://karpathy.github.io/2025/01/llm-os/
/kb ingest article "Karpathy 关于 LLM OS 的博客"
```

对于不方便抓取的平台（微信公众号、小红书），可以直接粘贴内容：

```
/kb ingest article [粘贴内容]
```

**做了什么：**
- 抓取网页内容（或接受粘贴的文本）
- 生成 5-section 文章笔记：核心观点、关键洞察、论据与案例、提到的论文、个人思考
- 识别文章中提到的论文，自动关联到 vault 中已有的 paper
- 标记值得 ingest 但尚未入库的论文
- 归类 topics 并更新 topic 文件
- 可选保存 snapshot.html 防止链接失效

### 1. Read — 精读标注一篇论文

```
/kb read papers/AlphaEvolve/AlphaEvolve.pdf
```

**做了什么：**
- 解析 PDF 全文，逐段分析论证结构
- 生成双栏 HTML：左侧原文 + 5 色高亮，右侧逐段批注（段落功能、逻辑角色、论证技巧）
- 底部输出论证结构总览 + APA/BibTeX 一键复制
- HTML 可直接在浏览器中打开

**问题驱动模式：**
```
/kb read paper.pdf --questions "Q1: 方法创新点? Q2: 和 baseline 比较?"
```
切换为按问题着色，底部生成 Q&A Worksheet。

**Tip：** 可以先 ingest 再 read 做深度精读，也可以先 read 再 ingest——如果已有精读标注，生成的 paper note 质量会更高。

### 2. Compile — 综合一个主题

```
/kb compile Agent Memory
```

**做了什么：**
- 读取该 topic 下所有 paper notes、article notes 和 section 深读笔记的关键 section
- 向你展示 3-5 个跨论文的核心洞见，等你确认方向
- 更新 topic 文件：框架对比表、收敛结论、辩论与张力、开放问题排序
- 执行 backlink audit（确保双向链接完整）
- 更新 Topics Index.md 的论文计数

```
/kb compile --all    # 编译所有 topics
```

### 3. Query — 跨论文研究问题

```
/kb query "Agent Memory 和 Agent RL 的交叉点在哪里？"
```

**做了什么：**
- 从 topic 文件、paper notes、article notes 和 section 深读笔记中找相关内容（不用 general knowledge）
- 综合回答，每个事实都有 `[[wikilink]]` 出处
- 答案存到 `queries/` 文件夹，好的答案可以 promote 成新 topic

### 4. Lint — 健康检查

```
/kb lint
```

**检查内容：**
- Orphan papers/articles（topics 字段指向不存在的 topic）
- Dead wikilinks（topic 文件引用不存在的 paper 或 article）
- Frontmatter 缺失（缺 title/authors/topics 等关键字段）
- Missing backlinks（paper/article 在某个 topic 下但 topic 文件没列它）
- Stale counts（Topics Index.md 的计数过期）
- Dead article URLs（文章链接失效，可选深度检查）

修复方案会逐条列出，你确认后才会执行。

### 5. Log — 查看操作历史

```
/kb log              # 最近 20 条
/kb log ingest       # 只看 ingest 记录
/kb log 2026-04      # 只看 4 月的记录
```

## Vault 目录结构

```
vault-root/
├── papers/                            ← 学术论文
│   ├── [Paper Title]/
│   │   ├── [Paper Title].md           ← paper note（12-section 中英双语）
│   │   ├── [Paper Title].pdf          ← 原文 PDF
│   │   ├── annotation.html            ← 可选，/kb read 产出
│   │   └── sections/                  ← 可选，长论文自动创建
│   │       ├── 01-Introduction.md     ← 逐章深读笔记
│   │       ├── 03-Methodology.md
│   │       └── ...
│   └── Paper Database.md              ← Dataview 自动查询仪表盘
├── articles/                          ← 博客、技术报告、社交媒体
│   ├── [Article Title]/
│   │   ├── [Article Title].md         ← article note（5-section 笔记）
│   │   └── snapshot.html              ← 可选，网页快照防链接失效
│   └── Article Database.md            ← Dataview 自动查询仪表盘
├── topics/                            ← 跨论文主题综合
│   ├── [Topic Name].md               ← topic 综合文件（聚合 papers + articles）
│   └── Topics Index.md               ← 中心 hub + mermaid 图谱
├── queries/                           ← 跨语料研究问答
│   └── [YYYY-MM-DD] [Question].md
└── kb-log.md                          ← 操作审计日志
```

### 各文件夹用途与关联

**`papers/`** — 每篇论文一个子文件夹，以论文原标题命名。包含 PDF 原文、12-section 中英双语笔记（背景、问题、方法、结果等）、可选的 `/kb read` HTML 精读标注，以及长论文自动生成的 `sections/` 逐章深读笔记。Paper note 的 `topics` frontmatter 字段将其链接到 `topics/` 文件，正文中的 wikilinks 连接到其他论文和主题。

**`articles/`** — 每篇文章一个子文件夹（博客、技术报告、社交媒体帖子）。包含 5-section 笔记（核心观点、关键洞察、论据、相关论文、个人思考）和可选的网页快照。与论文类似，`topics` frontmatter 链接到 `topics/` 文件，`recommended_papers` 创建到 `papers/` 的桥梁。

**`topics/`** — 每个文件综合了所有标记为该主题的论文和文章。不是简单列表，而是提供框架对比表、跨论文模式、辩论与张力、优先级排序的开放问题。`Topics Index.md` 作为中心 hub，包含论文计数和 mermaid 依赖图。Topic 文件通过 wikilinks 反向链接到各论文/文章。

**`queries/`** — 保存的跨语料研究问题答案。每个 query 文件通过 wikilinks 引用具体的论文、文章和主题。优质的 query 答案可以被提升为新 topic 或合并到已有 topic 中。

**`kb-log.md`** — 仅追加的操作审计日志，记录所有操作（ingest、compile、query、lint）及时间戳，方便追溯什么时候添加或修改了什么。

**关联方式：**

```
papers/ ←──topics 字段──→ topics/
   │                          ↑
   │ wikilinks            wikilinks
   ↓                          │
articles/ ←─topics 字段──→ topics/
                              ↑
                         queries/（引用 papers + topics）
```

每篇论文和文章通过 frontmatter 链接到 1-5 个 topic。Topic 文件聚合所有来源的洞见。Query 从三者中提取信息并回馈系统。每次 ingest 都会让知识图谱更密集。

## 完整工作流

```
/kb ingest paper.pdf           → paper note + sections/（长论文）+ topic 归类
/kb ingest article <url>       → article note + topic 归类
      ↓ (可选深度精读)
/kb read paper.pdf             → HTML 精读标注（浏览器打开）
      ↓ (积累资料后)
/kb compile "Topic Name"       → topic 综合分析（papers + articles + section insights）
      ↓
/kb query "研究问题"            → 跨语料研究问答
      ↓ (定期)
/kb lint                       → 健康检查 + 修复
```

## 推荐的 Obsidian 插件

- **Dataview**（必装）— Paper Database.md 和 Topics Index.md 依赖它实现自动查询
- **Graph View**（内置）— 可视化 papers 和 topics 之间的 wikilink 网络
- **Templater** — 可选，用于快速创建新 paper note 骨架

## 文件说明

```
academic-kb/
├── SKILL.md                         # 主 skill 文件（所有流程定义）
├── CHANGELOG.md                     # 版本历史
├── README.md                        # English version
├── README.zh.md                     # 中文版（本文件）
└── references/
    ├── template.css                 # 精读 HTML 的样式表（5色高亮 + 双栏布局）
    ├── paper-frontmatter.md         # Paper note 的 YAML frontmatter schema
    ├── paper-note-template.md       # Paper note 的 12-section 模板
    ├── section-note-template.md     # Section 深读笔记模板
    ├── article-frontmatter.md       # Article note 的 YAML frontmatter schema
    └── article-note-template.md     # Article note 的 5-section 模板
```
