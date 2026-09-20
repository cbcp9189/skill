# 对话归档 Markdown 模板（summary 模式）

> **仅 summary 模式使用。** 用户明确说「整理成摘要版」「知识库版」「格式化笔记」时才读此文件。
>
> 默认模式为 **verbatim 对话实录**，见 [`conversation-transcript-template.md`](conversation-transcript-template.md)。

Agent 写入 Obsidian 时按此结构**整理、提炼**对话内容。可根据对话实际内容省略空章节，但 **frontmatter + 摘要 + 关键结论** 必填。

## 模板

```markdown
---
title: "{对话主题}"
date: {YYYY-MM-DD}
tags:
  - ai-chat
  - cursor
  - {根据内容补充 1～3 个标签，如 java / skill / enjoyiot}
source: cursor-agent
---

# {对话主题}

> 归档时间：{YYYY-MM-DD HH:mm}（GMT+8）

## 摘要

{3～5 句话：这次对话要解决什么问题、最终结论是什么}

## 关键结论

- {结论 1}
- {结论 2}
- {结论 3}

## 对话精华

### {小节标题 1}

{该主题的讨论要点、方案、取舍。用完整句子，不要聊天记录体「用户：… Agent：…」}

### {小节标题 2}

{…}

## 代码与配置

{如有代码、curl、环境变量、目录结构，放在此处；敏感信息脱敏}

```bash
# 示例
export ENJOYIOT_USERNAME="***"
```

## 文件与路径

| 项 | 路径 |
|----|------|
| {描述} | `{路径}` |

## 待办 / 后续

- [ ] {如有未完成的行动项}

## 相关链接

- {对话中提到的 URL，如有}
```

## 写作原则

| 原则 | 说明 |
|------|------|
| **知识库导向** | 写成「以后翻得懂的技术笔记」，不是聊天流水账 |
| **保留干货** | API、Skill 结构、命令、决策理由要保留 |
| **删减寒暄** | 「好的」「谢谢」等客套不写入 |
| **合并重复** | 用户多次追问同一问题时合并成一条清晰说明 |
| **标签有用** | `tags` 用你 vault 里可能已有的分类词，便于 Obsidian 搜索 |
| **脱敏** | 密码、Token、secret、内网 IP 若需保留结构，值用 `***` |

## 标签建议

按对话内容选用（不必全加）：

| 标签 | 适用 |
|------|------|
| `skill` | Cursor Skill 编写 |
| `java` | Java 后台 |
| `api` | 接口对接 |
| `enjoyiot` | EnjoyIot 项目 |
| `obsidian` | 笔记工作流 |
| `架构` | 方案设计讨论 |

## 示例文件名与 title 对应

| 对话内容 | title | 文件名 |
|---------|-------|--------|
| 讨论 Skill 三层结构 | 工具编排型 Skill 写法 | `2026-07-02-工具编排型Skill写法.md` |
| EnjoyIot 登录与查设备 | EnjoyIot 设备查询 Skill | `2026-07-02-EnjoyIot设备查询Skill.md` |
