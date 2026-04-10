# Section Deep-Read Note Template

When a paper's sections are long and content-rich (triggered automatically or via `--deep` flag), ingest creates individual deep-read notes in `papers/[Paper Title]/sections/`. Each file follows this template.

## Filename Convention

`NN-Section-Name.md` where `NN` is the zero-padded section number from the original paper (e.g., `01-Introduction.md`, `03-Methodology.md`, `04-Experiments.md`). Use the paper's own section titles — do NOT rename them.

## Template

```markdown
---
parent: "[[Paper Title]]"
section_number: N
section_title: "Original Section Title"
page_range: "pp. X-Y"
tags: [section-note]
---

# [Section Title]

> [!info] 章节定位
> **在论文中的角色：** [这个章节在论文整体论证链中扮演什么角色，1-2 句话]
> **前置依赖：** [理解本章需要哪些前置知识或前面章节的内容]
> **与下一章的衔接：** [这个章节如何过渡到下一个章节]

---

## 精读摘要

[对本章内容的深度摘要。不是简单复述，而是提炼核心论证逻辑：作者在这个章节想说明什么、怎么论证的、得出了什么结论。根据章节长度 3-6 段。]

[对于方法论章节，需要详细描述：算法步骤、架构设计、关键公式的直觉解释、设计决策的 rationale。]

[对于实验章节，需要覆盖：实验设置、baselines、关键结果表格的解读、ablation 分析、surprising findings。]

[对于 related work 章节，按论文的分类方式组织，提炼每个 category 的核心方法和局限，指出本文与各 category 的关系。]

---

## 关键 Insight

> [!tip] Insight 1: [简短标题]
> [这个 insight 是什么，为什么重要，对你的研究有什么启发。2-3 句话。]

> [!tip] Insight 2: [简短标题]
> [...]

[提取 2-5 个 insight。好的 insight 应该是可迁移、可借鉴的认知收获，而非论文的简单重述。例如：]
[- 一个新颖的 problem formulation 角度]
[- 一个巧妙的实验设计技巧]
[- 一个反直觉的发现及其解释]
[- 一个可以迁移到自己研究中的方法或框架]
[- 一个值得质疑或进一步验证的 claim]

---

## 原文关键段落

> [!quote] [段落主题/要点的简短标签]
> [逐字引用原文中最重要的段落，保持原始语言（通常是英文）。选择标准：核心定义、关键发现、重要声明、精彩论证。]
>
> — Section N.M, p. X

> [!quote] [段落主题]
> [...]
>
> — Section N.M, p. X

[选择 2-4 个最有价值的段落。每段附出处标注（section 编号 + 页码）。]

---

## 与其他工作的联系

[本章内容与 vault 中其他论文/主题的关联。使用 wikilinks 链接。例如：]
[- 本章提出的方法与 [[Other Paper]] 的 approach 形成对比/互补]
[- 本章的发现支持/质疑了 [[Topic Name]] 中的某个共识]
[- 本章的实验设计可以借鉴到 [[Another Paper]] 的场景中]
[至少 1-2 个 wikilink。]
```

## Writing Guidelines

- **逐章精读，不是逐段翻译。** 目标是让读者不看原文也能深入理解这个章节的核心贡献和论证逻辑。
- **Insight 是核心价值。** 每个 insight 应该是可复用的知识点，而非论文特有的事实陈述。问自己："这个发现能启发我自己的研究吗？"
- **原文引用要精挑细选。** 只保留最精华的段落——定义性语句、核心发现、关键论证。不要大段引用。
- **Wikilinks 不可少。** 每个 section note 至少包含 1-2 个 wikilink 到相关论文或主题。
- **双语风格一致。** 分析内容用中文，技术术语、论文标题、专有名词保持英文。引用原文保持原始语言。

## Length Calibration

| Section type | Expected length |
|---|---|
| Introduction / Background | 300-500 words |
| Related Work | 400-600 words |
| Methodology / Architecture | 500-800 words |
| Experiments / Results | 400-700 words |
| Discussion / Conclusion | 200-400 words |

These are guidelines, not caps — complex methodology sections (e.g., 8+ pages in the paper) may warrant more.
