# Changelog

All notable changes to the academic-kb skill are documented here.

---

## 2026-04-10 — Section Deep-Read on Ingest

**New feature:** When ingesting long or complex papers, the skill now automatically creates per-section deep-read notes in a `sections/` subdirectory, enabling granular insight extraction beyond the main 12-section summary.

### Added

- **`references/section-note-template.md`** — New template for section-level deep-read notes. Each note contains five components:
  - 章节定位 (`[!info]` callout): section's role in the paper's argument chain, prerequisites, transition
  - 精读摘要: deep summary of core argument and methodology (3-6 paragraphs)
  - 关键 Insight (`[!tip]` callouts): 2-5 transferable, reusable insights per section
  - 原文关键段落 (`[!quote]` callouts): 2-4 verbatim key quotes with section/page references
  - 与其他工作的联系: cross-references to other papers/topics via wikilinks

- **Ingest flags: `--deep` / `--no-deep`** — Force or skip section-level deep-read regardless of automatic assessment.

- **Step 5b in Procedure 0 (Ingest)** — New step for deep-read assessment and section note generation:
  - Auto-trigger when: paper ≥ 12 pages, any section ≥ 3 pages, paper tagged `survey`, or relevance ≥ 4
  - Creates `papers/[Paper Title]/sections/NN-Section-Name.md` per original paper section
  - Skips trivial sections (Abstract, Acknowledgements, References, sections < 0.5 page)
  - Groups sub-sections under parent unless a sub-section itself spans ≥ 2 pages

- **Section embeds in main note** — When deep-read is active, each section in the main paper note includes a collapsed Obsidian embed:
  ```markdown
  > [!abstract]- 📖 深度精读笔记
  > ![[sections/03-Methodology]]
  ```

### Changed

- **Vault structure** — `sections/` subdirectory added as optional child of each paper folder.
- **`references/paper-note-template.md`** — Added "Deep-Read Section Embeds" section and updated writing guidelines to clarify that section notes complement (not replace) main note sections.
- **After Ingest summary** — Now reports deep-read status: whether section notes were created, count, and most insight-rich sections.
- **kb-log format** — Ingest entries now append `[+N section notes]` suffix when deep-read was triggered.
- **Section count label** — Fixed documentation from "10-section" to "12-section" (sections 0-11) to match actual template.

---

## 2026-04-09 — Bilingual Paper Notes

- Paper notes now use bilingual parallel format: Chinese first, then English, within every section.
- Updated `paper-note-template.md` with bilingual writing examples.

---

## 2026-04-08 — Initial Release

- Core pipeline: ingest → compile → query → lint.
- Paper ingest with 12-section structured notes and full frontmatter.
- Article ingest with 5-section notes for blogs, tech reports, social media.
- PDF annotation (`/kb read`) with dual-column HTML and 5-color highlights.
- Topic compilation with cross-paper synthesis.
- Corpus query with vault-grounded answers.
- Vault health lint with 8 checks.
