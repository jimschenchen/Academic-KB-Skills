# Idea Quick Template — 灵感速记模板

轻量版 idea note，用于快速捕捉灵感。与完整 Idea Template 的区别：只保留核心想法，省略可行性评估、深度思考等 section。

## 模板

```markdown
---
title: "{Title}"
tags:
  - idea
  - {project-tag}
date: {YYYY-MM-DD}
project: "[[projects/Project-{Name}/Project-{Name}]]"
status: seed
---

# {Title}

## 💡 核心想法

{用户的原始想法，1-3 句话，保留用户的措辞风格}

## 🔗 关联

- **项目**: [[projects/Project-{Name}/Project-{Name}|{Alias}]]
```

## 规则

1. **status 固定为 `seed`**：速记的想法都从 seed 开始
2. **project tag**：使用项目的简称小写形式作为 tag，如 `disasterdb`、`fgoals-agent`
3. **核心想法**：尽量保留用户的原始措辞，不要过度提炼或改写
4. **关联 section**：只写项目链接，不需要填论文和其他想法（日后展开时再加）
5. **如果用户提供了更多上下文**（为什么想到、初步思路等），可以追加：
   ```markdown
   ## 📝 备注

   {用户提供的额外上下文}
   ```

## 何时升级为完整模板

当用户说以下类似的话时，应该用完整的 `ideas/templates/Idea Template.md`：
- "详细记一下"
- "展开说说这个想法"
- "帮我分析一下可行性"
- 主动对某个 seed idea 进行深入讨论
