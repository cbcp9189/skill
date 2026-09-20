# 对话实录 Markdown 模板（verbatim 模式）

**仅 verbatim 模式使用。** summary 模式见 [`conversation-template.md`](conversation-template.md)。

Agent 写入 Obsidian 时，按当前会话**时间顺序**还原每一轮 User / Assistant 的**原文**，不做摘要、合并或改写。

## 模板

```markdown
---
title: "{短标题}"
date: {YYYY-MM-DD}
tags:
  - ai-chat
  - cursor
  - transcript
source: cursor-agent
archive_mode: verbatim
---

# 对话实录：{短标题}

> 归档时间：{YYYY-MM-DD HH:mm}（GMT+8）
> 模式：对话实录（非摘要）
> 敏感信息已脱敏（如有）

## 用户

{第 1 条用户消息，原文}

## Assistant

{第 1 条 Agent 回复，原文}

---

## 用户

{第 2 条用户消息，原文}

## Assistant

{第 2 条 Agent 回复，原文}

---

{继续直到当前会话最后一轮}
```

若因上下文长度导致最早几轮无法还原，在 frontmatter 下方追加：

```markdown
> ⚠️ 本实录可能不完整：最早若干轮因上下文限制未能还原。
```

## 写作规则

| 规则 | 说明 |
|------|------|
| **原文照搬** | 用户消息逐字保留，不改写、不润色、不合并多轮 |
| **轮次结构** | 每轮：`## 用户` → `## Assistant` → `---` |
| **Agent 回复** | 保留对用户可见的最终回复全文（含 markdown、代码块、表格） |
| **禁止摘要** | 不得出现「摘要」「关键结论」「对话精华」等整理型章节 |
| **禁止省略** | 不得用「（略）」「详见上文」「同上」代替长回复 |
| **保留提问线索** | 「好的」「帮我把…」「是不是…」等提问必须保留 |
| **不写工具日志** | 不写入 Shell 输出、Read/Write 工具调用、thinking |
| **脱敏** | 密码、Token、secret、refreshToken 的值改为 `***`；可保留字段名与 JSON 结构 |

## 标题与文件名

- `title`：从对话中提炼 3～8 字的短标题（仅用于文件名和笔记标题，**不**替代正文）
- 文件名：`YYYY-MM-DD-{主题}-实录.md`

## 示例片段

```markdown
## 用户

是不是大多数 skill 都是这种形式

## Assistant

**大体上是的**——在 Cursor 这类 Agent 里，**Skill 的主流形态就是「Markdown 说明书 + 元数据」**……

---

## 用户

好的，你帮我起草一个版本吧，我先把必要的信息给你

{用户粘贴的 curl 与 JSON，密码已脱敏为 ***}

## Assistant

{Agent 完整回复，含 Skill 目录结构与代码示例}
```

## 与 summary 模式的区别

| 维度 | verbatim（本模板） | summary |
|------|-------------------|---------|
| 用户提问 | 原文逐条保留 | 可合并、省略客套 |
| Agent 回复 | 全文保留 | 提炼为「对话精华」 |
| 结构 | Q&A 轮次 | 摘要 / 结论 / 待办 |
| 适用 | 跟着提问思路复习 | 快速查阅知识点 |
