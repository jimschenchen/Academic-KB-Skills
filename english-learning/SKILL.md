---
name: english-learning
description: "Spaced-repetition English learning system stored in Obsidian markdown. Use this skill whenever the user wants to: add new English words, sentences, or usage patterns to learn; review or quiz themselves on saved vocabulary; check what's due for review; get a study summary or progress report; or anything related to their English learning practice. Also trigger when the user says things like 'I don't know this word', 'add this to my vocab', 'quiz me', 'review English', 'what words should I review today', or shares English content they want to memorize."
---

# English Learning Skill — Spaced Repetition System

Help the user build and maintain a personal English vocabulary & expression bank, stored as Obsidian markdown notes. The system uses a spaced-repetition algorithm (based on SM-2) to schedule reviews at optimal intervals — right before the user is likely to forget.

## Core Concepts

**What gets stored:** Individual entries — each one is a word, phrase, sentence, or usage pattern the user wants to learn. Every entry lives in its own `.md` file inside `english-learning/` in the user's vault.

**How review scheduling works (SM-2 simplified):**

Each entry tracks three numbers in its frontmatter:
- `interval` — days until next review (starts at 1)
- `ease` — a multiplier reflecting how easy the item is for the user (starts at 2.5)
- `next_review` — the date when this item should next appear in a review session

After the user answers during a quiz, update these based on their self-rating (1–5):
- **1 (Forgot completely):** reset interval to 1, ease −0.3 (min 1.3)
- **2 (Mostly forgot):** reset interval to 1, ease −0.15
- **3 (Hard but recalled):** interval stays, ease −0.05
- **4 (Good):** interval × ease, ease unchanged
- **5 (Easy):** interval × ease × 1.2, ease +0.1

The first two reviews always use fixed intervals: 1 day → 3 days, regardless of rating. After that, the formula kicks in.

## Directory Structure

All files live under `english-learning/` at the vault root:

```
english-learning/
├── words/          # single words and short phrases
├── sentences/      # full sentences to memorize
├── usage/          # usage patterns, collocations, grammar points
└── dashboard.md    # progress overview (auto-generated)
```

Create these directories the first time the user adds an entry. If they already exist, use them as-is.

## Entry File Format

Each entry is a single markdown file. The filename should be the English content (sanitized for filesystem safety — replace `/`, `\`, `:`, etc. with `-`). Keep filenames short; truncate long sentences to ~50 chars.

```yaml
---
type: word | sentence | usage
english: "ephemeral"
chinese: "短暂的，转瞬即逝的"
pronunciation: "/ɪˈfem.ər.əl/"       # optional, for words
context: "The ephemeral beauty of cherry blossoms"  # optional example sentence
category: academic | daily | idiom | phrasal-verb | grammar | collocation
difficulty: 1                         # 1=new, increases with failed reviews
created: 2026-04-11
next_review: 2026-04-12
interval: 1
ease: 2.5
review_count: 0
review_history: []                    # list of {date, rating} objects
tags:
  - english-learning
---

## Notes

Any additional notes the user wants to keep — etymology, mnemonics, related words, common mistakes, etc.
```

### How to populate fields when the user adds content

The user will give you content in various ways — a single word, a sentence, a screenshot, or just say "I don't know what X means." Your job is to:

1. **Identify the type** — is it a word, sentence, or usage pattern?
2. **Provide the Chinese translation** — accurate and natural, not machine-literal
3. **Add pronunciation** for individual words (IPA format)
4. **Create a context sentence** if the user only gave a word — pick a natural, memorable example
5. **Categorize** — academic, daily, idiom, phrasal-verb, grammar, or collocation
6. **Analyze** — briefly explain in the Notes section why this word/expression is worth learning, any common mistakes Chinese speakers make with it, and related expressions

When the user gives you a batch of items, process them all and create separate files for each.

## Workflow: Adding New Entries

When the user shares new content to learn:

1. Parse what they gave you — could be a word, sentence, phrase, a paragraph with underlined words, etc.
2. For each item, create the entry file with all fields filled in
3. Save to the appropriate subdirectory (`words/`, `sentences/`, `usage/`)
4. Confirm what was added with a brief summary showing the English, Chinese translation, and when first review is scheduled
5. If the content is ambiguous (e.g., a word with multiple meanings), ask which meaning they encountered or store the most common one and note alternatives

## Workflow: Review Session

When the user asks to review (e.g., "quiz me", "review English", "复习", "考考我"):

1. **Find due items:** Read all entry files and collect those where `next_review ≤ today`. Sort by: overdue items first (oldest `next_review`), then by lowest `ease` (hardest items first).
2. **If nothing is due:** Tell the user when their next review is scheduled. Optionally offer to do an early practice round with recently-added items.
3. **Run the quiz:** Present items one at a time using varied question formats (see Quiz Formats below). Mix up the formats to keep it engaging.
4. **After each answer:** 
   - Tell the user if they were right or wrong
   - Show the correct answer with explanation if wrong
   - Ask them to self-rate: 1–5 (or use a simpler "forgot / hard / good / easy" scale mapped to 1/3/4/5)
   - Update the entry's frontmatter: recalculate `interval`, `ease`, `next_review`, increment `review_count`, append to `review_history`
5. **Session summary:** After all due items are reviewed (or the user wants to stop), show a summary: how many reviewed, how many correct, which ones to watch out for.

## Quiz Formats

Choose the format based on entry type and what tests deeper recall. Vary formats across a session — don't use the same type twice in a row.

### 1. English → Chinese (英译中)
Show the English word/sentence, ask for the Chinese meaning.
```
What does "ephemeral" mean?
> 你的答案：______
```
Best for: words, phrases

### 2. Chinese → English (中译英)
Show the Chinese, ask the user to produce the English.
```
"短暂的，转瞬即逝的" 用英文怎么说？
> 你的答案：______
```
Best for: words (tests active recall, harder than recognition)

### 3. Fill in the Blank (填空)
Use the context sentence with the key word/phrase blanked out.
```
Complete the sentence:
"The _______ beauty of cherry blossoms reminds us to appreciate the present moment."
```
Best for: words with strong context sentences, collocations

### 4. Sentence Making (造句)
Give the user a word/phrase and ask them to write a sentence using it.
```
Use "notwithstanding" in a sentence:
> 你的句子：______
```
Then evaluate their sentence — is it grammatically correct? Does it use the word appropriately? Give feedback.
Best for: usage patterns, academic words, grammar points

### 5. Multiple Choice (选择题)
Present 4 options (1 correct + 3 plausible distractors).
```
What does "serendipity" mean?
A) 巧合，意外发现的美好事物
B) 持续性的悲伤
C) 对未来的焦虑
D) 重复性的行为
```
Best for: early-stage learning (review_count < 3), giving the user confidence. Use sparingly for well-known items — it's too easy once you know the answer.

### 6. Error Correction (纠错)
Present a sentence with a deliberate error involving the target word/pattern.
```
Is this sentence correct? If not, fix it:
"I am looking forward to see you tomorrow."
```
Best for: grammar points, common mistakes, usage patterns

### 7. Contextual Usage (情景使用)
Describe a scenario and ask which learned expression fits.
```
你想在论文中表达"尽管有这些限制，我们的方法仍然有效"，用英文怎么说？
```
Best for: academic expressions, daily phrases, testing real-world application

## Workflow: Progress Dashboard

When the user asks for progress or a summary ("我学了多少了", "progress", "dashboard"):

1. Read all entry files
2. Generate `english-learning/dashboard.md` with:
   - Total entries by type and category
   - Items due today / overdue / upcoming this week
   - Recently added items
   - "Trouble words" — items with ease < 2.0 or high fail rate
   - Learning streak (consecutive days with reviews)
3. Show a brief summary in conversation too

## Workflow: Browse and Organize

When the user wants to see their collection ("看看我存了什么", "list my vocabulary"):

1. Read entries and group by category or type
2. Present a clean list with English, Chinese, and current mastery level
3. Offer to filter by: category, difficulty, date added, due status

## Important Behaviors

**Language of interaction:** The user is a Chinese speaker learning English. Use Chinese for explanations, instructions, and feedback. Use English for the actual learning content. This mix is natural and helpful for language learning.

**Be encouraging but honest:** When the user gets something wrong, explain clearly why, but don't be discouraging. When they get it right, acknowledge it briefly without being over-the-top.

**Analyze deeply:** When adding a word/sentence, don't just give a translation — explain nuance, register (formal/informal), common collocations, and mistakes Chinese speakers often make. This context makes the word stick better.

**Batch operations:** The user might paste a whole paragraph and say "help me learn the hard words." Parse it, identify the challenging vocabulary, and create entries for each.

**Flexible input:** Accept content in any format — a single word, a sentence, a screenshot, a paragraph, an article excerpt, even "I heard someone say X, what does it mean?" Always create the entry and add it to the system.

## References

- See [references/sm2-algorithm.md](references/sm2-algorithm.md) for the full SM-2 algorithm details and edge cases
- See [references/quiz-templates.md](references/quiz-templates.md) for more quiz format examples and selection logic
