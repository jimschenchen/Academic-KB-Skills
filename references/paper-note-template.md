# Paper Note Template

This is the standard structure for paper notes in `papers/[Paper Title]/[Paper Title].md`. The frontmatter is defined in `paper-frontmatter.md`. Below the frontmatter, every paper note follows this 10-section structure.

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

[Explain the problem this paper addresses in plain language that a smart non-expert
could understand. What real-world problem does this connect to? Why should anyone
care? 2-4 paragraphs.]

---

## 2. Background / Motivation（面向领域内读者）

[Technical background for someone already in the field. What's the state of the art?
What specific technical limitations motivated this work? How does this paper position
itself relative to existing work? Include references to prior work and technical
concepts. 2-4 paragraphs.]

---

## 3. Problem Statement / Research Question

[Clearly state the research questions. Use numbered list for multiple RQs.
Be precise about what the paper claims to solve.]

---

## 4. Prior Work / Limitation

[What existing approaches exist? What are their specific limitations?
Organize by approach type or chronologically. Be concrete about what
each prior work can and cannot do.]

---

## 5. Gap

[What specific gap does this paper fill? How does it differ from everything
in section 4? This should be a crisp 1-3 paragraph statement.]

---

## 6. Key Idea / Insight

[The core innovation or insight. What is the "aha" moment? This is often
the most valuable section — capture the intellectual contribution, not
just the technical implementation.]

---

## 7. Approach Overview

[Technical approach: architecture, algorithm, methodology. Use sub-sections
for complex systems. Include diagrams (ASCII or mermaid) where helpful.
Reference specific figures/tables from the paper if useful.]

---

## 8. Contributions

[Structured list of claimed contributions. Match what the paper claims,
then add your assessment of each.]

---

## 9. Results / Evidence

[Key experimental results. What benchmarks? What baselines? What metrics?
Highlight surprising or important numbers. Note any limitations in the
evaluation setup.]

---

## 10. Discussion / Limitations

[What does this mean for the field? What are the paper's own acknowledged
limitations? What limitations does the paper NOT acknowledge? What are
the open questions this work raises?]
```

## Writing Guidelines

- **Depth over breadth**: Each section should be substantive, not just a sentence. Aim for 2-4 paragraphs per section, more for complex papers.
- **Bilingual style**: Write primarily in Chinese for analytical content, use English for technical terms, proper nouns, and method names.
- **Inline wikilinks**: Link to relevant topics (`[[Agent Memory]]`) and other papers in the vault (`[[Hu-2026-Memory-Age-AI-Agents|Hu 2026]]`) throughout the text.
- **Critical thinking**: Don't just summarize — analyze. Note strengths, weaknesses, connections to other work in the vault.
- **HTML annotation leverage**: If an HTML annotation from `/paper read` exists, its paragraph-level analysis (论证功能, 逻辑角色, 论证技巧) is gold for enriching sections 1-10. Use the structural analysis to write more insightful notes.
