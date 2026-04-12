# Quiz Format Templates and Selection Logic

## Format Selection Matrix

Choose the quiz format based on entry type, review stage, and variety needs:

| Entry Type | Early Stage (count < 3) | Mid Stage (3-6) | Mature (7+) |
|------------|------------------------|------------------|-------------|
| word | Multiple Choice, EN→CN | CN→EN, Fill Blank | Sentence Making, Contextual |
| sentence | EN→CN, Error Correction | CN→EN, Fill Blank | Contextual Usage |
| usage | Multiple Choice, Error Correction | Fill Blank, Sentence Making | Contextual Usage, CN→EN |

**Variety rule:** Never use the same format for two consecutive questions in a session. Cycle through at least 3 different formats per session.

**Difficulty progression:** As review_count increases, shift from recognition (multiple choice) to production (sentence making, contextual usage). Production is harder but builds deeper knowledge.

## Detailed Templates

### English → Chinese (英译中)

**For words:**
```
📝 这个词是什么意思？

**ephemeral**

> 请给出中文意思：______
```

**For sentences:**
```
📝 翻译这个句子：

"The implications of this finding are far-reaching."

> 你的翻译：______
```

After the user answers, show:
- ✅ or ❌
- The correct translation
- If wrong: explain the key difference

---

### Chinese → English (中译英)

```
📝 用英文怎么说？

"尽管如此" (用于学术写作)

> 你的答案：______
```

Accept synonyms! If the stored answer is "nevertheless" but the user says "nonetheless" or "notwithstanding," that's correct too. List acceptable alternatives after grading.

---

### Fill in the Blank (填空)

```
📝 填入合适的词：

"The researchers _______ that the effect was statistically significant."
(提示：第一个字母是 c)

> 你的答案：______
```

**Hint strategy:** 
- First attempt: no hint
- If the user asks for a hint or says 不知道: give first letter
- Second hint: give word length and first+last letter

---

### Sentence Making (造句)

```
📝 用 "notwithstanding" 造一个句子：

> 你的句子：______
```

**Evaluation criteria:**
1. Grammar — is the sentence grammatically correct?
2. Usage — is the word/phrase used appropriately?
3. Naturalness — does it sound like something a native speaker would write?
4. Context — is the register appropriate (academic word in academic context)?

Give specific, actionable feedback. Not just "good" or "wrong" — explain what could be improved.

Example feedback:
```
✅ 语法正确！用法也很恰当。

你写的：Notwithstanding the limitations, our study provides valuable insights.
评价：非常好！这是学术论文中的典型用法。"notwithstanding" 放在句首引导让步状语，后面接名词短语，非常地道。

💡 小建议：你也可以用 "Notwithstanding" 放在句末：
"Our study provides valuable insights, notwithstanding the limitations."
两种位置都很常见。
```

---

### Multiple Choice (选择题)

```
📝 "Serendipity" 是什么意思？

A) 巧合，意外发现的美好事物
B) 对某事物的强烈渴望
C) 深深的失望感
D) 一种焦虑状态

> 你的选择：______
```

**Distractor generation rules:**
- Distractors should be plausible (same part of speech, similar register)
- At least one distractor should be a common confusion for Chinese learners
- Don't make distractors obviously wrong or humorous
- Shuffle the position of the correct answer

---

### Error Correction (纠错)

```
📝 这个句子有错吗？如果有，请改正：

"I am looking forward to see you at the conference."

> 你的答案：______
```

**Answer format:** Show the corrected sentence, highlight the change, and explain the rule.

```
❌ 有错误！

正确：I am looking forward to **seeing** you at the conference.
解释："look forward to" 中的 "to" 是介词，不是不定式标志，所以后面要跟动名词 (-ing)。
这是中国学生非常常见的错误，因为看到 "to" 就习惯性地接动词原形。

📌 记忆技巧：把 "look forward to" 当成一个固定搭配整体记忆，就像 "be used to doing" 一样。
```

---

### Contextual Usage (情景使用)

```
📝 情景题：

你在写论文的 Discussion 部分，需要表达"我们的结果与之前的研究一致"。
用英文怎么说？

> 你的答案：______
```

Accept multiple valid expressions. After grading, show 2-3 alternatives:

```
✅ 很好！你的表达完全正确。

你写的：Our results are consistent with previous studies.
其他同样地道的表达：
- "Our findings align with prior research..."
- "These results corroborate earlier findings by..."
- "Our observations are in line with..."

💡 在学术写作中，用不同的表达方式能让文章更丰富，避免重复。
```

## Session Flow Template

A typical review session follows this pattern:

1. **Opening:** "你今天有 N 个词/句要复习。准备好了吗？" 
2. **Questions:** Present items one by one with varied formats
3. **After each question:** Grade, feedback, self-rating
4. **Every 5 questions:** Quick progress update ("已经复习了 5/15 个，正确率 80%")
5. **Closing summary:**
   ```
   📊 本次复习总结：
   - 复习数量：15 个
   - 正确率：80% (12/15)
   - 需要注意的：ephemeral, notwithstanding, collocation
   - 下次复习：明天有 5 个词到期
   ```

## Adaptive Difficulty

If the user gets 3+ items right in a row, consider:
- Switching to harder formats (production over recognition)
- Adding follow-up questions ("Can you use this word in a sentence too?")

If the user gets 3+ items wrong in a row, consider:
- Switching to easier formats (multiple choice, giving more hints)
- Slowing down and spending more time on explanation
- Asking "要不要先暂停，我帮你整理一下这几个容易混淆的词？"
