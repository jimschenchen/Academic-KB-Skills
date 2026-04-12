# Article Note Frontmatter Schema

Every article note in `articles/[Article Title]/[Article Title].md` must begin with this YAML frontmatter. Fill as many fields as possible during ingest.

```yaml
---
title: "Article Title"
author: "Author Name"                # single author or primary author
author_affiliation: "Google DeepMind" # company, university, or "independent"
date: 2026-04-08                     # publish date (YYYY-MM-DD)
source_url: "https://..."            # original URL (critical — the primary reference)
source_type: blog                    # blog / tech-report / white-paper / social-media / newsletter
platform: "personal blog"            # personal blog / Medium / Substack / 微信公众号 / 小红书 / Twitter/X / GitHub / company blog / other
tags:
  - article                          # ALWAYS first tag
  - your-domain-tags                 # e.g., agent, LLM, tutorial
topics:
  - Topic Name A                     # PLAIN TEXT, must match filenames in topics/
  - Topic Name B
status: unread                       # unread / read / archived
relevance: 3                         # 1 (low) – 5 (high) for user's research
summary: "一句话总结文章核心观点"
recommended_papers:                  # papers mentioned or recommended in this article
  - "[[Paper Title 1]]"
  - "[[Paper Title 2]]"
---
```

## Field Notes

**source_url**: The most important field for articles. Unlike papers which have PDFs, articles live on the web. Always preserve the original URL.

**source_type values**:
- `blog` — personal or company blog posts (Karpathy's blog, Google AI blog, etc.)
- `tech-report` — technical reports, not formally published (e.g., internal reports released publicly)
- `white-paper` — company white papers, industry reports
- `social-media` — 小红书, 微信公众号, Twitter/X threads, etc.
- `newsletter` — email newsletters, Substack posts

**platform**: Free text, but use consistent names. Common values: `personal blog`, `Medium`, `Substack`, `微信公众号`, `小红书`, `Twitter/X`, `GitHub`, `company blog` (Google AI Blog, OpenAI Blog, etc.)

**recommended_papers**: Wikilinks to papers in `papers/` that the article mentions, reviews, or recommends. This creates a bridge between the articles and papers sections of the vault.

**topics field**: Same as papers — plain text matching filenames in `topics/`. This is what allows topic files to aggregate insights from both papers and articles.

**relevance field**: Same scale as papers:
- 5: Directly relevant to current research, contains actionable insights
- 4: Closely related, provides useful perspectives or paper recommendations
- 3: Related topic, good for broadening understanding
- 2: Tangentially related
- 1: Peripheral, saved for reference
