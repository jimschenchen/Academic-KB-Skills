---
name: academic-kb
description: "Academic knowledge base manager for Obsidian vaults — ingest papers and articles into structured notes, compile topic syntheses, query across the corpus, and lint for health. Use this skill whenever the user mentions /kb, wants to add a paper or article to their vault, asks to summarize or synthesize a research topic, wants to compile or update topic notes, asks cross-paper research questions, or mentions ingesting, compiling, linting, or organizing academic papers. Also trigger when the user says things like 'add this paper', 'add this article', 'update the topic', 'what do my papers say about X', or 'check my vault for broken links'."
---

# Academic Knowledge Base

Manage an Obsidian-based academic knowledge base with a full pipeline: **ingest → compile → query → lint**. This skill turns raw PDFs into structured paper notes, captures articles (blogs, tech reports, white papers, social media) as lightweight notes, synthesizes cross-source insights into topic files, answers research questions grounded in the corpus, and keeps the vault healthy.

## Vault Structure

The vault follows this layout — never deviate from it:

```
vault-root/
├── papers/
│   ├── [Paper Title]/                # one folder per paper, using the original full title
│   │   ├── [Paper Title].md          # structured paper note
│   │   ├── [Paper Title].pdf         # original PDF
│   │   ├── annotation.html           # optional, from /kb read
│   │   └── sections/                 # optional, auto-created for long/complex papers
│   │       ├── 01-Introduction.md    # deep-read note per section
│   │       ├── 03-Methodology.md
│   │       └── ...
│   └── Paper Database.md             # Dataview dashboard (auto-queries, do not edit manually)
├── articles/
│   ├── [Article Title]/              # one folder per article, using the original title
│   │   ├── [Article Title].md        # structured article note (5-section)
│   │   └── snapshot.html             # optional, web page snapshot for link rot prevention
│   └── Article Database.md           # Dataview dashboard (create if missing)
├── papers_topics/
│   ├── [Topic Name].md               # topic synthesis file (aggregates both papers and articles)
│   └── Topics Index.md               # central hub with mermaid map
├── papers_queries/                    # cross-corpus research Q&A (create if missing)
│   └── [YYYY-MM-DD] [Question Slug].md
└── kb-log.md                         # append-only audit trail (create at vault root if missing)
```

## Related Skills

This skill orchestrates several companion skills — use them for sub-tasks:

- **[obsidian-markdown](../obsidian-markdown/SKILL.md)** — author paper notes and topic files with valid Obsidian Flavored Markdown (wikilinks, callouts, properties).
- **[obsidian-cli](../obsidian-cli/SKILL.md)** — interact with the running Obsidian vault (open notes, search, refresh).

## Subcommands

| Command | Usage | Description |
|---------|-------|-------------|
| `ingest` | `/kb ingest <pdf-path-or-query>` | Download (if needed) + parse PDF → structured paper note with full frontmatter, 12-section analysis, and optional deep-read section notes |
| `ingest article` | `/kb ingest article <url-or-text>` | Capture a blog/report/social-media post → structured article note with 5-section analysis |
| `read` | `/kb read <pdf-path>` | Annotate a PDF → full dual-column HTML with 5-color highlights |
| `compile` | `/kb compile <topic>` or `/kb compile --all` | Synthesize all papers and articles under a topic → update topic file |
| `query` | `/kb query "<question>"` | Answer a cross-corpus research question → save to papers_queries/ |
| `lint` | `/kb lint` | Health check: orphan papers/articles, dead wikilinks, stale topics, frontmatter gaps |
| `log` | `/kb log` | Show recent operations from kb-log.md |

**Flags (for `read`):**
- `--questions "Q1:... Q2:..."` — switch to Question mode (Mode A)
- `--questions` (no arg) — interactive: Claude asks you for questions
- `--lang en|zh` — override annotation language (default: `zh`)

**Flags (for `ingest`):**
- `--deep` — force section-level deep-read notes (creates `sections/` subdirectory)
- `--no-deep` — skip section-level deep-read even for long papers

---

## Procedure 1: Read / Annotate a Paper

**Input:** A PDF file path. Optional flags: `--questions`, `--lang`.

This produces a self-contained dual-column HTML annotation — original text on the left with color highlights, analytical annotations on the right. The output goes to `papers/[Paper Title]/annotation.html` and can later be consumed by `ingest` for richer paper notes.

### Mode Resolution

- No `--questions` → **Mode B** (logic analysis, default)
- `--questions "Q1: ... Q2: ..."` → **Mode A** (question-driven, inline)
- `--questions` (no argument) → **Mode A** (interactive: ask user for up to 6 questions)

### Steps

1. **Read the PDF** at the provided path. Extract: title, authors, year, venue/journal, abstract, full text.

2. **Determine the paper title** and output path. Save to `papers/[Paper Title]/annotation.html`. If the papers folder doesn't exist yet, create it and copy the PDF there.

3. **Generate the full dual-column HTML annotation.** Embed `references/template.css` as a `<style>` block in `<head>`. Import Google Fonts: Lora + IBM Plex Sans.

### HTML Document Structure

```
1. TOP NAVBAR
   - Paper title
   - Context label (or "General Reading" if none)
   - Color legend: 5 colored chips with dimension labels

2. STICKY SECTION NAV
   - Links: Abstract | Introduction | Related Work | Methods | Results | Discussion | Conclusion
   - Highlight active section on scroll

3. DUAL-COLUMN BODY — one <div class="paragraph-group"> per paragraph
   Left column (.original-text) — original text (ALWAYS IN ORIGINAL LANGUAGE, never translate):
     - Copy verbatim from PDF
     - Highlight key phrases: <mark class="thesis">...</mark> etc.
   Right column (.annotation-card) — annotation cards (language follows --lang, default zh):
     [Colored left border matching paragraph's dominant highlight]
     ① 段落功能 / Paragraph Function
     ② 逻辑角色 / Logical Role
     ③ 论证技巧或潜在漏洞 / Rhetorical Technique or Logical Gap

4. BACK-TO-TOP BUTTON
   <button id="back-to-top" title="返回顶部">↑</button>
   <script>
     const btn = document.getElementById('back-to-top');
     window.addEventListener('scroll', () => {
       btn.style.display = window.scrollY > 300 ? 'flex' : 'none';
     });
     btn.addEventListener('click', () => window.scrollTo({ top: 0, behavior: 'smooth' }));
   </script>

5. BOTTOM — Argument Structure Overview (language follows --lang)
   zh: 问题 / 论点 / 证据 / 反驳处理 / 结论
   en: Problem / Argument / Evidence / Concession / Conclusion
   - Author's core claim (1 sentence)
   - 最强论证 / Strongest argument
   - 最弱论证 / Weakest argument
   - APA Citation + copy button
   - BibTeX block + copy button

   <script>
   function copyText(btn, text) {
     navigator.clipboard.writeText(text);
     btn.textContent = '✓ 已复制'; btn.classList.add('copied');
     setTimeout(() => {
       btn.textContent = btn.dataset.label; btn.classList.remove('copied');
     }, 1500);
   }
   </script>
```

### 5-Color Highlight System (Mode B)

| Color | CSS Class | Represents |
|-------|-----------|-----------|
| 🟡 Yellow `#fef08a` | `thesis` | Core thesis / main claim |
| 🔴 Red `#fecaca` | `concept` | Key concepts / terminology |
| 🔵 Blue `#bfdbfe` | `evidence` | Empirical evidence / data |
| 🟢 Green `#bbf7d0` | `concession` | Concessions / counterargument handling |
| 🟣 Purple `#e9d5ff` | `methodology` | Methodology description |

### Mode A: Question-Driven (triggered by --questions)

Same HTML structure as Mode B, with these differences:
- Color legend: per-question colors Q1–Q6 (cycling through a distinct palette)
- Left highlights: `<mark class="q1">`, `<mark class="q2">`, etc.
- Right annotation cards labeled `【Q2 核心论点】` / `[Q2 Core Argument]`:
  ① Paragraph Function ② Argument Logic ③ Which question this paragraph answers
- Bottom: **Q&A Worksheet** (replaces Argument Structure Overview):
  One card per question — Core argument + Key evidence + Potential counter/limitation

### CSS

All CSS is in `references/template.css`. When generating HTML output, embed the file contents as a `<style>` block in `<head>`.

### After Read

Output: file path + brief summary of key arguments found. The HTML file can be opened directly in any browser.

---

## Procedure 0: Ingest a Paper

**Input:** One of the following:
- A **local PDF path** — e.g., `papers/AlphaEvolve-2025/AlphaEvolve-2025.pdf`
- An **arXiv ID or URL** — e.g., `2502.13131` or `https://arxiv.org/abs/2502.13131`
- A **paper title or description** — e.g., `"AlphaEvolve: A coding agent for scientific discovery"`
- A **DOI or Semantic Scholar URL** — e.g., `10.xxxx/xxxxx`

Optionally, an HTML annotation from `/kb read` may already exist in the same folder.

### Step 0 (if input is not a local PDF): Download the paper

1. **Resolve the paper identity:**
   - If arXiv ID or URL → extract the arXiv ID directly
   - If DOI → resolve to a download URL
   - If paper title/description → search for it:
     - First try arXiv API: `https://export.arxiv.org/api/query?search_query=ti:"<title>"&max_results=5`
     - If not found, try OpenAlex: `https://api.openalex.org/works?search=<query>&select=title,doi,primary_location&per-page=5`
     - Present the top results to the user for confirmation before downloading

2. **Download the PDF:**
   - For arXiv: `https://arxiv.org/pdf/<arxiv_id>.pdf`
   - For DOI with open access: follow the DOI to the publisher, look for PDF link
   - Save to `papers/[Paper Title]/[Paper Title].pdf` (create the folder if needed)
   - If the download fails (paywall, unavailable), inform the user and ask them to provide the PDF manually

3. **Continue to Step 1** with the downloaded PDF path.

### Steps

1. **Determine the paper title.** Use the original full title of the paper as the folder and file name. Examples: `Agentic Reasoning`, `Memory in the Age of AI Agents`. Keep the exact title from the paper — do not abbreviate or reformat.

2. **Create the paper folder** at `papers/[Paper Title]/`. Copy or note the PDF path. If an HTML annotation already exists (from a prior `/kb read`), check for it at `papers/[Paper Title]/annotation.html` or in the same directory as the PDF.

3. **Extract paper content.** Read the PDF to extract: title, authors, affiliations, year, venue, abstract, full text, and references.
   - If an HTML annotation exists, read it too — it contains pre-analyzed paragraph functions, argument structure, and key highlights that significantly speed up step 5.

4. **Fill the frontmatter.** Use the exact schema from `references/paper-frontmatter.md`. Every field matters for Dataview queries. Key rules:
   - `tags`: always include `paper` as the first tag, then domain tags
   - `topics`: plain text array (NOT wikilinks) — e.g., `[Agent Memory, Agent RL]`. Only use topic names that exist in `papers_topics/`, or propose a new one and flag it to the user
   - `status`: set to `unread` initially (the user will change it after reading)
   - `relevance`: make your best estimate (1-5) based on the user's research focus, but tell the user your reasoning so they can adjust
   - `summary`, `research_question`, `contributions`, `findings`: fill these from the paper content

5. **Write the 12-section paper note (sections 0-11).** Follow the template in `references/paper-note-template.md`. The sections are:
   - 0: Publication Info
   - 1: Background / Motivation (general audience)
   - 2: Background / Motivation (domain experts)
   - 3: Problem Statement / Research Question
   - 4: Prior Work / Limitation
   - 5: Gap
   - 6: Key Idea / Insight
   - 7: Approach Overview
   - 8: Contributions
   - 9: Results / Evidence
   - 10: Discussion / Limitations
   - 11: Conclusion / Future Work

   Each section should be substantive (not just one sentence). **Bilingual format is required**: write each section with Chinese first, then English below (separated by a blank line), so both languages appear side-by-side within every section. Technical terms and proper nouns stay in English in both versions. If an HTML annotation is available, leverage its highlights and argument analysis to enrich sections 1-11.

   **Section embed for deep-read papers:** If step 5b created section notes, add an Obsidian embed at the end of each corresponding section in the main note:
   ```markdown
   > [!abstract]- 📖 深度精读笔记
   > ![[sections/03-Methodology]]
   ```
   Use a collapsed callout so the main note stays scannable, but readers can expand to see the full deep-read inline.

5b. **Deep-read assessment and section note generation.** Evaluate whether this paper warrants section-level deep-read notes. This step produces individual notes per original paper section in `papers/[Paper Title]/sections/`.

   **Trigger logic (evaluated automatically unless overridden by `--deep` / `--no-deep`):**
   - Count the number of distinct sections in the paper (Introduction, Related Work, Methods, Experiments, etc.)
   - Estimate total paper length (page count or word count)
   - A paper qualifies for deep-read if **any** of these hold:
     - Total paper length ≥ 12 pages (excluding references)
     - Any single section spans ≥ 3 pages
     - The paper is tagged as `survey` or the user's `relevance` estimate is ≥ 4
     - The `--deep` flag was explicitly provided
   - Skip if `--no-deep` was provided or the paper is very short (≤ 6 pages)

   **If deep-read is triggered:**

   a. **Create the `sections/` directory** at `papers/[Paper Title]/sections/`.

   b. **Identify sections to deep-read.** Map the paper's original section structure (e.g., "1 Introduction", "2 Related Work", "3 Method", "3.1 Architecture", "3.2 Training", "4 Experiments", "5 Discussion"). For papers with deep sub-section hierarchies, group at the top-level section (e.g., "3 Method" covers 3.1–3.N in one note) unless a sub-section is itself very long (≥ 2 pages), in which case give it its own note.

   c. **For each section, create a deep-read note** following the template in `references/section-note-template.md`. The filename follows `NN-Section-Name.md` (e.g., `01-Introduction.md`, `03-Methodology.md`). Each note contains:
      - **章节定位 callout**: the section's role in the paper's argument chain, prerequisites, and transition to the next section
      - **精读摘要**: deep summary of the section's core argument, methodology details, or experimental findings (3-6 paragraphs, scaled to section length)
      - **关键 Insight**: 2-5 transferable insights extracted from this section (using `[!tip]` callouts)
      - **原文关键段落**: 2-4 verbatim quotes of the most valuable paragraphs (using `[!quote]` callouts with section/page references)
      - **与其他工作的联系**: connections to other papers/topics in the vault via wikilinks

   d. **Frontmatter for each section note:**
      ```yaml
      ---
      parent: "[[Paper Title]]"
      section_number: 3
      section_title: "Methodology"
      page_range: "pp. 4-8"
      tags: [section-note]
      ---
      ```

   e. **Skip trivial sections.** Do not create section notes for: Abstract (already in frontmatter), Acknowledgements, purely bibliographic Reference lists, or sections shorter than half a page. The Publication Info (section 0) in the main note already covers metadata.

   f. **After generating all section notes,** inform the user:
      - How many section notes were created
      - Which sections were skipped and why
      - Highlight the 2-3 most insight-rich sections

6. **Weave wikilinks throughout the note — this is critical for Obsidian's graph and backlink features.** The paper note should contain at least 5-10 wikilinks spread across sections 1-10. Two types:

   - **Topic links**: Every topic listed in the frontmatter `topics` field should appear as `[[Topic Name]]` at least once in the body text where that topic is most relevant. For example, if `topics: [Agent Memory, Agent RL]`, then section 2 or 6 might say "This paper's memory mechanism is closely related to recent advances in [[Agent Memory]]".
   - **Paper links**: When the paper cites or relates to other papers already in the vault, link them as `[[Paper Title|display text]]`. Check `papers/` for existing paper folders. For example: "Compared to the Agentic Reasoning framework by [[Agentic Reasoning|Wei et al. 2026]]..."

   A paper note with zero topic/paper wikilinks is incomplete — the whole point of the knowledge base is cross-linking. Scan the vault's existing papers for related work and link generously.

7. **Classify into topics.** Based on the paper's content, suggest 2-5 topics from the existing `papers_topics/` directory. If a paper clearly belongs to a topic that doesn't exist yet, propose creating it (but don't create it automatically — ask the user first).

8. **Update topic files.** For each topic in the paper's `topics` field, open the corresponding topic file and add a wikilink to the new paper in its `相关论文` section. Use the format: `- [[Paper Title|Short Description]] — one-line summary of relevance`.

9. **Update Topics Index.md.** If any paper count changed, update the count in the Topics table.

10. **Append to kb-log.md:**
    ```
    ## [YYYY-MM-DD] ingest | [Paper Title] ([venue], [year]) [+N section notes]
    ```
    The `[+N section notes]` suffix is only added when deep-read was triggered. Omit it for papers without section notes.

### After Ingest

Present a summary to the user:
- Paper title
- Topics assigned (with reasoning)
- Relevance score (with reasoning)
- Deep-read status: whether section notes were created, how many, and which sections are most insight-rich
- Any proposed new topics
- Any fields left blank that the user should fill

---

## Procedure 0b: Ingest an Article

**Input:** One of the following:
- A **URL** — e.g., `https://karpathy.github.io/2025/01/llm-os/`
- **Pasted text** — user pastes content directly (common for 小红书, 微信公众号 where scraping is hard)
- A **title + platform hint** — e.g., `"Karpathy 的 LLM OS 博客"` (Claude searches for it)

### Step 0: Acquire the content

1. **If URL is provided:**
   - Fetch the web page content. Extract the main article body, title, author, date, and platform.
   - If fetching fails (paywall, anti-scraping), ask the user to paste the content manually.
   - Optionally save a snapshot to `articles/[Article Title]/snapshot.html` for link rot prevention.

2. **If pasted text is provided:**
   - Ask the user for the source URL (if they have it), author, and date.
   - Proceed with the text as-is.

3. **If title/description is provided:**
   - Search the web for the article. Present top results for user confirmation before proceeding.

### Steps

1. **Determine the article title.** Use the original title of the article. For social media posts without a clear title, create a descriptive one: `[Author] on [Topic] ([Platform], [Date])`. Examples: `LLM OS`, `The Bitter Lesson`, `Karpathy on AutoResearch (Twitter/X, 2026-03)`.

2. **Create the article folder** at `articles/[Article Title]/`.

3. **Fill the frontmatter.** Use the exact schema from `references/article-frontmatter.md`. Key rules:
   - `tags`: always include `article` as the first tag
   - `source_url`: critical — always include the original URL if available
   - `source_type`: one of `blog`, `tech-report`, `white-paper`, `social-media`, `newsletter`
   - `platform`: specific platform name (e.g., `personal blog`, `微信公众号`, `小红书`, `Medium`)
   - `topics`: same as papers — plain text matching `papers_topics/` filenames
   - `recommended_papers`: wikilinks to papers the article mentions. Check `papers/` for existing titles.

4. **Write the 5-section article note.** Follow the template in `references/article-note-template.md`:
   - 1: Core Argument / 核心观点
   - 2: Key Insights / 关键洞察
   - 3: Evidence & Examples / 论据与案例
   - 4: Related Papers / 提到的论文
   - 5: My Take / 个人思考 (leave empty or minimal — user fills in later)

   Scale depth to article length: a 小红书 post might produce a 200-word note, a long-form blog post 600-800 words.

5. **Weave wikilinks throughout the note.** Same as paper ingest — at least 3-5 wikilinks:
   - **Topic links**: `[[Topic Name]]` for each topic in the frontmatter
   - **Paper links**: `[[Paper Title|display text]]` for papers the article references that exist in the vault
   - **Article links**: `[[Article Title|display text]]` for other articles in the vault if related

6. **Classify into topics.** Suggest 1-3 topics from existing `papers_topics/`. Articles may also suggest papers worth ingesting — flag these to the user.

7. **Update topic files.** For each topic in the article's `topics` field, add a wikilink in the topic file's `相关论文与资源` section. Use a 📝 marker to distinguish from papers: `- 📝 [[Article Title|Short Description]] — one-line summary`.

8. **Append to kb-log.md:**
    ```
    ## [YYYY-MM-DD] ingest article | [Article Title] ([platform])
    ```

### After Ingest

Present a summary to the user:
- Article title and source
- Topics assigned
- Papers mentioned (which are in vault, which are potential ingest candidates)
- Relevance score

---

## Procedure 2: Compile a Topic

**Input:** A topic name (must match a file in `papers_topics/`), or `--all` to compile every topic.

Compile reads all papers and articles tagged with that topic and updates the topic synthesis file. The goal is NOT to rewrite from scratch every time — it's to incrementally improve the synthesis as new papers are added.

### Steps

1. **Load the topic file** from `papers_topics/[Topic Name].md`.

2. **Find all papers and articles for this topic.** Scan `papers/` and `articles/` for all `.md` files where frontmatter `topics` contains this topic name (case-insensitive match).

3. **Read each paper and article note.** For papers, focus on sections 3 (Problem Statement), 6 (Key Idea), 7 (Approach), 8 (Contributions), 9 (Results), 10 (Discussion). For articles, focus on sections 1 (Core Argument), 2 (Key Insights), 4 (Related Papers). You don't need to re-read the full PDFs or source pages — the notes are the source of truth.

4. **Surface key insights before writing.** Present to the user:
   - How many papers are in this topic (new since last compile, if known from kb-log.md)
   - 3-5 key cross-paper insights or patterns
   - Any contradictions between papers
   - Any gaps (aspects of the topic not covered by any paper)

   Ask: "Anything specific to emphasize or de-emphasize?" Wait for response before proceeding. Skip this step only if the user explicitly says to compile autonomously.

5. **Update the topic file.** Preserve the existing structure:

   ```markdown
   ---
   title: "Topic: [Name]"
   tags: [topic, topic-slug]
   date: [today]
   related_topics: ["[[Other Topic]]", ...]
   ---

   # [Topic Name]

   > [!abstract] 主题概述
   > [2-4 sentences overview — what this topic is about, why it matters,
   >  key papers driving it]

   ---

   ## 核心问题 (Core Questions)
   [Key open questions with paper references]

   ---

   ## Insight Synthesis：[Framework/Analysis Title]
   [Cross-paper analysis — the heart of the topic file]
   [Use tables for framework comparisons]
   [Use ASCII diagrams for architecture patterns]
   [Include subsections for tensions/debates]

   ---

   ## 相关论文与资源
   [Wikilinks to papers and articles in this topic, grouped if useful]
   [Use 📄 for papers, 📝 for articles]

   ---

   ## 与其他主题的关联
   [Links to related topics with explanation]
   ```

   **Target length: 1500-2500 words** for the synthesis body (not counting paper lists and frontmatter). This is the intellectual core of the knowledge base — invest the effort here.

   **What makes a good synthesis** (look at `papers_topics/Agent Memory.md` as the gold standard):
   - **Framework comparison tables** — if multiple papers propose frameworks or taxonomies, build a table comparing their dimensions side by side, then analyze the convergences and divergences beneath the table
   - **Convergent conclusions** — what do the papers agree on? Cite specific evidence from each
   - **Tensions and debates** — where do papers disagree or take different approaches? Name the tension explicitly (e.g., "internal vs. external memory trade-off") and present both sides with citations
   - **Recurring architectural patterns** — if you see the same design pattern emerging across papers, name it and draw an ASCII diagram
   - **Open questions ranked by urgency** — what does the collective body of work NOT answer yet?

   A compile that merely lists "Paper A says X, Paper B says Y, Paper C says Z" is a failure. The value is in the cross-cutting analysis that no single paper provides. Think of yourself as a researcher writing a mini-survey for a colleague who needs to get up to speed on this topic in 10 minutes.

6. **Backlink audit.** After updating the topic:
   - Check every paper note in this topic — does it contain an inline wikilink to `[[Topic Name]]`? If not, add one at the first relevant mention.
   - Check related topics — does the `related_topics` field and the "与其他主题的关联" section reflect current connections?

7. **Update Topics Index.md.** Update the paper count for this topic. If the mermaid diagram's connections changed, update those too.

8. **Append to kb-log.md:**
    ```
    ## [YYYY-MM-DD] compile | [Topic Name] ([N] papers, [word_count] words)
    ```

### Compile --all

When `--all` is specified, iterate through every `.md` file in `papers_topics/` (excluding `Topics Index.md`). Compile each topic in sequence. At the end, do a global update of Topics Index.md with all counts.

---

## Procedure 3: Query the Corpus

A query answers a research question by reading the vault — never from general knowledge. The answer is then filed back so the exploration compounds.

### Phase A — Answer from the vault

1. **Identify relevant topics.** Scan `papers_topics/` for topic files whose content relates to the question. Read their `主题概述` and `Insight Synthesis` sections.

2. **Identify relevant papers and articles.** From the topic files and from grepping frontmatter in both `papers/` and `articles/`, find sources that address the question. For papers, read sections 3, 6, 7, 8, 9, 10. For articles, read sections 1, 2, 4.

3. **Synthesize the answer.** Properties:
   - Every factual claim traces back to a `[[Paper Title]]` or `[[Topic Name]]` citation
   - Note agreements and disagreements between papers
   - Flag gaps explicitly: "No paper in the vault covers X" or "[[Topic]] does not yet discuss Y"
   - Suggest follow-up ingest targets (papers that should be added)

4. **Match format to question type:**
   - Factual → prose with inline wikilink citations
   - Comparison → table with paper citations in cells
   - How-it-works → numbered steps with citations
   - What-do-we-know → structured "Known / Open Questions / Gaps"

### Phase B — File back

5. **Save the answer** to `papers_queries/[YYYY-MM-DD] [Question Slug].md` with frontmatter:
   ```yaml
   ---
   title: "[Question]"
   tags: [query, relevant-tags]
   date: YYYY-MM-DD
   topics: [Topic1, Topic2]
   informed_by:
     - "[[Paper Title 1]]"
     - "[[Paper Title 2]]"
     - "[[Article Title 1]]"
   ---
   ```

6. **Consider promoting.** If the answer is a durable synthesis (comparison table, trade-off analysis, new concept), suggest to the user that it could become a new topic or be merged into an existing topic file.

7. **Append to kb-log.md:**
    ```
    ## [YYYY-MM-DD] query | [Question Slug]
    ```

---

## Procedure 4: Lint and Heal

Run a health check across the entire vault. Report issues and propose fixes.

### Checks

1. **Orphan papers/articles** — entries in `papers/` or `articles/` whose `topics` field is empty or contains only topics that don't exist in `papers_topics/`.

2. **Dead wikilinks in topic files** — topic files that reference papers or articles that don't exist in `papers/` or `articles/`.

3. **Dead wikilinks in notes** — paper/article notes that link to `[[Topic Name]]` where no such file exists in `papers_topics/`.

4. **Frontmatter gaps** — papers missing critical fields (`title`, `authors`, `year`, `topics`, `status`, `tags`); articles missing critical fields (`title`, `author`, `source_url`, `topics`, `tags`).

5. **Stale topic counts** — Topics Index.md counts don't match actual Dataview-style query results (count both papers and articles).

6. **Missing backlinks** — papers/articles assigned to a topic (via frontmatter `topics`) but not listed in that topic's `相关论文与资源` section.

7. **Unlinked entries in topics** — topic files list papers/articles in `相关论文与资源` that aren't in those entries' `topics` frontmatter (asymmetric link).

8. **Dead article URLs** — articles whose `source_url` returns 404 or is unreachable (optional, only if user requests deep lint). Suggest saving a snapshot if none exists.

### Output

For each issue, print the problem and a proposed fix. Group by severity:
- 🔴 **Broken** — dead links, missing files
- 🟡 **Incomplete** — missing frontmatter, missing backlinks
- 🟢 **Suggestion** — stale counts, potential new topic connections

Ask the user: "Should I auto-fix these issues?" Apply fixes only with permission.

### After lint

Append to kb-log.md:
```
## [YYYY-MM-DD] lint | [N] issues found, [M] fixed
```

---

## Procedure 5: View Log

Show recent operations from `kb-log.md`.

```bash
grep "^## \[" kb-log.md | tail -20
```

Support filters:
- `/kb log ingest` — show only ingest events
- `/kb log compile` — show only compile events
- `/kb log 2026-04` — show events from April 2026

---

## Language Conventions

The vault uses a **bilingual (中英双语) style**. Every paper note section is written twice — Chinese first, then English — within the same section. This ensures the vault is useful for both Chinese-native and English-native readers.

- **Frontmatter**: always English (for Dataview queryability)
- **Section headers**: English with Chinese annotation where conventional (e.g., `## 1. Background / Motivation（面向大众读者）`)
- **Section body — bilingual parallel format**: Write the Chinese version first (2-4 paragraphs), then leave a blank line, then write the English version (2-4 paragraphs). Both versions should convey the same content but each should read naturally in its own language — do NOT produce a word-for-word translation. Technical terms, method names, paper titles, and proper nouns remain in English in both versions.
- **Section headers in topic files**: Chinese with English in parentheses (e.g., `## Insight Synthesis`, `## 相关论文与资源`)
- **Wikilinks**: always use the note's filename (the original full paper title), optionally with display text: `[[Memory in the Age of AI Agents|Hu 2026]]`

## Error Handling

- **PDF unreadable**: output error with path, suggest checking file integrity
- **URL unreachable**: inform user, suggest pasting content manually or trying later
- **Topic not found**: list existing topics and suggest closest match
- **Paper/article already exists**: warn and ask whether to overwrite or skip
- **kb-log.md missing**: create it at the vault root with a header `# Knowledge Base Log`
- **papers_queries/ missing**: create the directory automatically
- **articles/ missing**: create the directory automatically
- **Ambiguous topic assignment**: present candidate topics with confidence levels and let the user decide
