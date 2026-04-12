# SM-2 Spaced Repetition Algorithm

## Overview

This is a simplified version of the SuperMemo SM-2 algorithm, adapted for this English learning system. The goal: show items right before the user would forget them, maximizing retention while minimizing review time.

## Parameters per Entry

| Field | Default | Description |
|-------|---------|-------------|
| `interval` | 1 | Days until next review |
| `ease` | 2.5 | Ease factor — how quickly intervals grow |
| `review_count` | 0 | Total number of times reviewed |
| `next_review` | created + 1 day | Next scheduled review date |
| `review_history` | [] | Array of `{date, rating}` records |

## Rating Scale

The user rates their recall after each quiz question:

| Rating | Label | Meaning |
|--------|-------|---------|
| 1 | 完全忘了 (Forgot) | No memory at all |
| 2 | 大部分忘了 (Mostly forgot) | Vague recognition but couldn't recall |
| 3 | 费劲想起来了 (Hard) | Recalled with significant effort |
| 4 | 想起来了 (Good) | Recalled correctly with some thought |
| 5 | 很简单 (Easy) | Instant recall, no effort needed |

For a simpler UX, map a 4-option scale:
- 忘了 → 1
- 费劲 → 3  
- 记得 → 4
- 太简单了 → 5

## Algorithm Steps

After the user rates an item:

### Step 1: Update ease factor

```
if rating == 1: ease = max(1.3, ease - 0.3)
if rating == 2: ease = max(1.3, ease - 0.15)
if rating == 3: ease = max(1.3, ease - 0.05)
if rating == 4: ease = ease  (no change)
if rating == 5: ease = ease + 0.1
```

### Step 2: Calculate new interval

```
if rating <= 2:
    # Failed — reset to beginning
    interval = 1
elif review_count == 0:
    # First successful review
    interval = 1
elif review_count == 1:
    # Second successful review
    interval = 3
else:
    # Subsequent reviews
    interval = round(interval * ease)
    if rating == 5:
        interval = round(interval * 1.2)  # bonus for easy
```

### Step 3: Update next_review

```
next_review = today + interval days
```

### Step 4: Update review_count

Only increment `review_count` if rating >= 3 (successful recall). If rating < 3, reset `review_count` to 0 — the user needs to relearn this item.

### Step 5: Record in review_history

Append `{date: today, rating: N}` to the review_history array.

## Edge Cases

**Overdue items:** If an item is overdue by many days and the user still recalls it, that's a strong signal. Apply a small bonus: `ease += 0.05` for each week overdue (max +0.2).

**New items with no review yet:** `next_review` is set to the day after creation. If the user adds items at night, they'll see them the next day.

**Leeches:** Items reviewed more than 8 times with an ease below 1.5 are "leeches" — the user keeps forgetting them. Flag these in the dashboard and suggest the user add a mnemonic or rephrase their notes.

**Maximum interval:** Cap intervals at 180 days. Even well-known items should be refreshed twice a year.

**Minimum ease:** Never let ease drop below 1.3. At this floor, intervals still grow, just slowly.

## Review Order

When multiple items are due, present them in this order:
1. Overdue items (oldest first)
2. Items with lowest ease (hardest first)
3. New items (added today, getting first review)
4. Items due today sorted by review_count ascending (less-reviewed first)

## Session Sizing

A typical review session should be 10-20 items. If more than 20 items are due:
- Review the 20 most urgent (by the ordering above)
- Tell the user how many remain for later
- Offer to continue if they want
