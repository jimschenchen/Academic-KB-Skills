# Article Note Template

This is the standard structure for article notes in `articles/[Article Title]/[Article Title].md`. The frontmatter is defined in `article-frontmatter.md`. Below the frontmatter, every article note follows this 5-section structure.

## Template

```markdown
# [Article Title]

> [!info] Article Links
> - Source: [Article Title](source_url)
> - Author: Author Name (Affiliation)
> - Date: YYYY-MM-DD
> - Platform: platform name

---

## 1. Core Argument / 核心观点

[What is the author's main point or thesis? Distill the central message in 1-3 paragraphs.
For social media posts, this may be very short. For long-form blog posts or white papers,
capture the main thread of the argument.]

---

## 2. Key Insights / 关键洞察

[What are the most valuable takeaways? List 3-7 key insights, each with a brief explanation.
Focus on ideas that are novel, actionable, or connect to your research.
Use wikilinks to connect to relevant topics: [[Topic Name]].]

---

## 3. Evidence & Examples / 论据与案例

[What evidence or examples does the author provide? Benchmarks, case studies, demos,
anecdotes? How convincing are they? Note any gaps in evidence.]

---

## 4. Related Papers / 提到的论文

[Papers mentioned, cited, or recommended in this article. Use wikilinks for papers
already in the vault. For papers not yet in the vault, note the full title and
mark with ⬜ as potential ingest candidates.]

- [[Paper Title|display text]] — context of how the article discusses this paper
- ⬜ **Paper Not Yet Ingested** (Author, Year) — why it's worth adding

---

## 5. My Take / 个人思考

[Your own reflection: how does this connect to your research? Does it change how
you think about a topic? Any follow-up actions (papers to read, experiments to try)?
This section is personal and can be filled in later.]
```

## Writing Guidelines

- **Conciseness**: Articles vary wildly in length. A 小红书 post note might be 200 words total; a long-form tech report note might be 800 words. Scale proportionally.
- **Bilingual style**: Same as paper notes — Chinese for analytical content, English for technical terms and proper nouns.
- **Wikilinks are essential**: Link to topics (`[[Agent Memory]]`) and papers (`[[Paper Title|display text]]`) throughout. The whole point is connecting articles to the academic knowledge base.
- **Section 4 is the bridge**: The Related Papers section is what ties articles back to your papers collection. Be thorough here — if an article mentions a paper, check whether it's in your vault.
- **Section 5 can be empty at ingest**: Unlike sections 1-4 which should be filled during ingest, "My Take" is personal reflection that the user may add later.
