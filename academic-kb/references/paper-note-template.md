# Paper Note Template

This is the standard structure for paper notes in `papers/[Paper Title]/[Paper Title].md`. The frontmatter is defined in `paper-frontmatter.md`. Below the frontmatter, every paper note follows this 12-section structure (0-11) in **bilingual (中英双语) format**.

## Bilingual Format Rule

Each section body is written **twice** — Chinese first, then English — separated by a blank line. Both versions convey the same content but each reads naturally in its own language (not word-for-word translation). Technical terms, method names, paper titles, and proper nouns stay in English in both versions.

## Template

```markdown
# [Full Paper Title]

> [!info] Paper Links
> - PDF: ![[Paper Title.pdf]]
> - arXiv: [arXiv ID](https://arxiv.org/abs/XXXX.XXXXX)

---

## 0. Publication Info

**Title:** Full title
**Authors & Affiliations:** Author1 (Affiliation)¹, Author2 (Affiliation)², ...
**Venue:** Conference/Journal Name, Year
**Date:** Month Day, Year
**arXiv ID:** XXXX.XXXXX
**Keywords:** keyword1, keyword2, ...

---

## 1. Background / Motivation（面向大众读者）

[中文版本：用通俗易懂的语言解释论文要解决的问题。这个问题与什么现实场景相关？
为什么值得关注？2-4 段。]

[English version: Explain the problem this paper addresses in plain language
that a smart non-expert could understand. What real-world problem does this
connect to? Why should anyone care? 2-4 paragraphs.]

---

## 2. Background / Motivation（面向领域内读者）

[中文版本：面向领域内读者的技术背景。当前 SOTA 是什么？
什么技术瓶颈驱动了这项工作？论文在现有工作中的定位？
引用先前工作和技术概念。2-4 段。]

[English version: Technical background for someone already in the field.
What's the state of the art? What specific technical limitations motivated
this work? Include references to prior work. 2-4 paragraphs.]

---

## 3. Problem Statement / Research Question

[中文版本：清晰陈述研究问题。多个 RQ 用编号列表。
精确描述论文声称要解决什么。]

[English version: Clearly state the research questions. Use numbered list
for multiple RQs. Be precise about what the paper claims to solve.]

---

## 4. Prior Work / Limitation

[中文版本：现有方法有哪些？各自的具体局限是什么？
按方法类型或时间线组织。具体说明每个先前工作能做什么、不能做什么。]

[English version: What existing approaches exist? What are their specific
limitations? Organize by approach type or chronologically.]

---

## 5. Gap

[中文版本：论文填补了什么具体空白？与 section 4 中的工作有何不同？
简洁的 1-3 段陈述。]

[English version: What specific gap does this paper fill? How does it differ
from everything in section 4? A crisp 1-3 paragraph statement.]

---

## 6. Key Idea / Insight

[中文版本：核心创新或洞察。"aha moment" 是什么？
捕捉智力贡献，不仅仅是技术实现。]

[English version: The core innovation or insight. What is the "aha" moment?
Capture the intellectual contribution, not just the technical implementation.]

---

## 7. Approach Overview

[中文版本：技术方案：架构、算法、方法论。复杂系统用子 section。
必要时用图表（ASCII 或 mermaid）。引用论文中的具体图表。]

[English version: Technical approach: architecture, algorithm, methodology.
Use sub-sections for complex systems. Include diagrams where helpful.]

---

## 8. Contributions

[中文版本：论文声称的贡献结构化列表。匹配论文原文声称，
然后加上你的评估。]

[English version: Structured list of claimed contributions. Match what the
paper claims, then add your assessment of each.]

---

## 9. Results / Evidence

[中文版本：关键实验结果。什么 benchmark？什么 baseline？什么指标？
突出令人惊讶或重要的数字。注意评估设置中的局限性。]

[English version: Key experimental results. What benchmarks? What baselines?
What metrics? Highlight surprising or important numbers.]

---

## 10. Discussion / Limitations

[中文版本：这对该领域意味着什么？论文自认的局限是什么？
论文未承认的局限是什么？这项工作提出了哪些开放问题？]

[English version: What does this mean for the field? What are the paper's
acknowledged limitations? What limitations does the paper NOT acknowledge?]

---

## 11. Conclusion / Future Work

[中文版本：论文的主要结论是什么？提出了什么未来方向？
最有前景的后续步骤是什么？这项工作可能如何演进？1-3 段。]

[English version: What are the paper's main conclusions? What future directions
does it suggest? What are the most promising next steps? 1-3 paragraphs.]
```

## Deep-Read Section Embeds

When a paper triggers deep-read (see SKILL.md Step 5b), each section that has a corresponding deep-read note in `sections/` should include a collapsed embed callout at the end of the section:

```markdown
## 7. Approach Overview

[中文版本：... 2-4 段 ...]

[English version: ... 2-4 paragraphs ...]

> [!abstract]- 📖 深度精读笔记
> ![[sections/07-Methodology]]
```

This keeps the main note scannable while giving readers one-click access to the full deep-read analysis. Only add embeds for sections that actually have a corresponding section note — not every section will have one (trivial sections are skipped).

## Writing Guidelines

- **Depth over breadth**: Each section should be substantive, not just a sentence. Aim for 2-4 paragraphs per language per section, more for complex papers.
- **Bilingual parallel format**: Chinese first, English second, within the same section. Both versions should be independently readable — not a mechanical translation.
- **Inline wikilinks**: Link to relevant topics (`[[Agent Memory]]`) and other papers in the vault (`[[Paper Title|Short Display]]`) throughout both language versions.
- **Critical thinking**: Don't just summarize — analyze. Note strengths, weaknesses, connections to other work in the vault.
- **HTML annotation leverage**: If an HTML annotation from `/kb read` exists, its paragraph-level analysis (论证功能, 逻辑角色, 论证技巧) is gold for enriching sections 1-11. Use the structural analysis to write more insightful notes.
- **Section notes complement, not replace, main sections**: When deep-read is active, the main note sections still provide the high-level summary. Section notes go deeper with insights, key quotes, and cross-references. Don't make the main note section thinner just because a section note exists.
