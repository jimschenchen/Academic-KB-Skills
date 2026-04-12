# Paper Note Frontmatter Schema

Every paper note in `papers/[Paper Title]/[Paper Title].md` must begin with this YAML frontmatter. All fields are important for Dataview queries — fill as many as possible during ingest.

```yaml
---
title: "Full Paper Title"
authors:
  - First Author
  - Second Author
  # or shorthand: [First Author, et al.]
year: 2024
venue: "NeurIPS"              # arXiv / CHI / ACL / EMNLP / ICML / ICLR / etc.
venue_type: "A* Conference"   # One of: A* Conference / Conference / Journal / Workshop / Preprint
doi: "10.xxxx/xxxxx"          # optional, omit if unavailable
arxiv: "2411.xxxxx"           # optional, omit if no arXiv version
url: "https://..."            # primary URL (arXiv, DOI, or publisher page)
tags:
  - paper                     # ALWAYS first tag
  - survey                    # paper type: survey / empirical / theoretical / system / benchmark
  - your-domain-tags          # e.g., agent-memory, LLM, reinforcement-learning
status: unread                # unread / reading / read / archived
read_date:                    # YYYY-MM-DD, fill when user marks as read
relevance: 3                  # 1 (low) – 5 (high) for user's research
topics:
  - Topic Name A              # PLAIN TEXT, not wikilinks. Must match filenames in topics/
  - Topic Name B
keywords:
  - keyword1
  - keyword2
affiliations:
  - University A
  - Company B
summary: "一句话总结论文核心贡献"
research_question: "论文要解决的主要研究问题"
contributions:
  - "贡献1：描述"
  - "贡献2：描述"
findings:
  - "发现1：描述"
  - "发现2：描述"
---
```

## Field Notes

**topics field**: This is the critical link between papers and topic files. Use exact topic names that match files in `topics/`. The Dataview query in Paper Database.md uses `FLATTEN topics AS topic` to group papers by topic.

**tags field**: First tag is always `paper`. Second tag should indicate paper type (survey, empirical, system, etc.). Remaining tags are domain-specific and should be lowercase, hyphenated.

**venue_type field**: Use these exact values for consistent filtering:
- `A* Conference` — top-tier: CHI, NeurIPS, ICML, ICLR, ACL, EMNLP, SIGIR, CSCW, CVPR, AAAI
- `Conference` — other peer-reviewed conferences
- `Journal` — peer-reviewed journals
- `Workshop` — workshop papers
- `Preprint` — arXiv or other preprint servers

**relevance field**: Rate based on how directly the paper relates to the user's active research:
- 5: Directly addresses user's core research question
- 4: Closely related, provides important methods or insights
- 3: Related topic, useful for background or comparison
- 2: Tangentially related
- 1: Peripheral, included for completeness
