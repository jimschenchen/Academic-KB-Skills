# Skills Dev — Custom Skill Development Repository

Source code for self-developed Claude Skills. Each skill is a standalone folder containing SKILL.md (core instructions), references/ (templates and reference materials), and documentation.

To deploy, copy the skill folder to `.claude/skills/`.

---

## Three Skills at a Glance

### 1. academic-kb — Academic Version of Karpathy's LLM Wiki

A full-pipeline skill for managing an Obsidian-based academic knowledge base: ingest → read → compile → query → lint. Supports both academic papers (PDF) and articles (blogs, tech reports, white papers, social media).

Inspired by:

* **Karpathy's knowledge base workflow** — the core idea of an ingest → compile → query → lint pipeline
* **Paperwise (sjqsgg)** — dual-column HTML annotation design with 5-color highlight system

**Core capabilities:**
- Paper Ingest: PDF → 12-section bilingual structured note + optional section-level deep-read notes
- Article Ingest: URL/text → 5-section note
- Topic Compile: cross-paper/article synthesis on a given topic
- Cross-corpus Query: research Q&A across the entire knowledge base
- Vault Lint: detect orphan notes, broken links, missing fields
- Operation Log: all changes tracked in kb-log.md

**Data under management:** 46 paper notes, 6 article notes, 11 topic syntheses, 3 research queries

**File structure:**
```
academic-kb/
├── SKILL.md              # Core instructions (~600 lines, 6 Procedures)
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

### 2. project-manager — Academic Project Lifecycle Management

Manages research projects, funding proposals, idea capture, and the relationship network between them.

**Core capabilities:**
- Create projects / fundings with standardized frontmatter and templates
- Relationship management: project ↔ funding / idea / paper / project (bidirectional)
- Status updates with dependency checks (downstream impact alerts, incomplete task warnings)
- Progress reports (per-project / per-funding / global overview)
- Auto-sync Funding-Project Mapping
- Vault structural health check (6-dimensional Lint)
- Idea capture: mention a thought, auto-classify to the right project

**Data under management:** 9 projects, 7 funding proposals, 8+ idea notes, interconnected via .base database views and Mapping index

**File structure:**
```
project-manager/
├── SKILL.md              # Core instructions (~470 lines, 8 Procedures)
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

### 3. english-learning — Spaced Repetition English Learning System

SM-2-based English vocabulary and expression learning system, stored in Obsidian markdown.

**Core capabilities:**
- Add words/sentences/usage patterns: auto-generates Chinese translation, pronunciation, context sentence, study notes
- Spaced review: SM-2 algorithm scheduling with 7 quiz formats (EN→CN, CN→EN, fill-in-the-blank, sentence making, multiple choice, error correction, contextual usage)
- Adaptive difficulty: dynamically adjusts quiz format based on accuracy
- Progress dashboard: learning stats, trouble words, review schedule
- Batch import: paste a paragraph, auto-extract difficult vocabulary

**Data under management:** entry files in words/, sentences/, usage/ subdirectories

**File structure:**
```
english-learning/
├── SKILL.md              # Core instructions (~210 lines, 4 Workflows)
└── references/
    ├── sm2-algorithm.md   # Full SM-2 algorithm implementation
    └── quiz-templates.md  # Quiz format templates & selection logic
```

---

## How the Skills Work Together

```
academic-kb ──paper links──→ project-manager
    │                             │
    │ vocabulary from papers       │ terminology from ideas
    ↓                             ↓
          english-learning
```

- **academic-kb → project-manager**: papers link to projects via wikilinks in Related Notes
- **academic-kb → english-learning**: unfamiliar words encountered while reading papers can be added directly to the learning system
- **project-manager → academic-kb**: project notes reference papers, but paper management (ingest, deep-read) is handled by academic-kb

---

## Dev → Deploy

```bash
# Deploy a skill to the Claude Skills directory
cp -r skills-dev/{skill-name}/ .claude/skills/{skill-name}/
```

Skills take effect immediately in Cowork / Claude Code after deployment.
