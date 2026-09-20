---
name: obsidian-chat-archive
version: 1.1.0
description: "将  对话写入 Obsidian 知识库。默认按轮次原样保存用户提问与 Agent 回复（对话实录）；仅当用户明确要求摘要/知识库版/格式化笔记时才整理成结构化笔记。触发词：保存到 Obsidian、对话实录、原样保存、整理成摘要版。"
metadata:
  requires:
    bins: []
---

# Obsidian 对话归档

把**当前会话**写入 Obsidian 仓库。**默认保存对话实录**（原样 Q&A），不是摘要笔记。

## 模式路由

| 模式 | 何时使用 | 模板 |
|------|----------|------|
| **verbatim（默认）** | 用户未明确要求摘要；或说「存到 Obsidian」「保存对话」「对话实录」「原样保存」「帮我把对话存到笔记」 | [`references/conversation-transcript-template.md`](references/conversation-transcript-template.md) |
| **summary（可选）** | 用户明确说「整理成摘要」「知识库版」「格式化笔记」「整理成摘要版存 Obsidian」 | [`references/conversation-template.md`](references/conversation-template.md)（**仅 summary 模式读此文件**） |

**未说明模式时，一律 verbatim。**

## 路径配置

| 变量 | 默认值 | 说明 |
|------|--------|------|
| `OBSIDIAN_VAULT_PATH` | `D:\obsidian_file\pc_note` | Obsidian 仓库根目录 |
| `OBSIDIAN_AI_NOTES_DIR` | `AI对话` | 存放 AI 归档笔记的子目录（相对 vault 根） |

**完整保存路径：**

```
{OBSIDIAN_VAULT_PATH}/{OBSIDIAN_AI_NOTES_DIR}/{文件名}.md
```

示例（实录）：`D:\obsidian_file\pc_note\AI对话\2026-07-02-Skill编写与EnjoyIot接口-实录.md`

若用户指定子目录（如「存到 10-Projects/Java」），则写入 `{OBSIDIAN_VAULT_PATH}/10-Projects/Java/{文件名}.md`。

## 执行流程

### verbatim 模式（默认）

```
1. 确定模式 → verbatim
2. 读取 conversation-transcript-template.md
3. 从当前会话上下文，按时间顺序列出每一轮 User / Assistant
4. 用户消息：原文照搬，不改写、不合并
5. Agent 消息：保留对用户可见的最终回复原文（含代码块、列表、表格）
6. 提炼短标题 → 生成文件名（带 -实录 后缀）
7. 敏感信息脱敏（密码、Token、secret → ***），文首注明
8. Write 落盘
9. 回复用户：路径 + 轮次数 + 模式名
```

### summary 模式

```
1. 确定模式 → summary（用户明确要求）
2. 读取 conversation-template.md
3. 通读对话，提炼主题 → 生成标题与文件名
4. 按摘要模板整理 Markdown
5. Write 落盘
6. 回复用户：路径 + 标题 + 一句话摘要
```

**BLOCKING — 必须使用 Write 工具落盘；禁止只在聊天里输出 Markdown 而不写文件。**

**BLOCKING — 写入前用 Glob 或 Read 检查目标路径是否已存在；若存在，文件名加 `-2`、`-3` 后缀，或询问用户是否覆盖。**

## verbatim 模式 CRITICAL 规则

- **禁止**摘要、合并多轮、改写用户提问
- **禁止**输出「对话精华 / 关键结论 / 摘要」等结构化整理章节
- **必须**按轮次交替：`## 用户` → `## Assistant` → `---`
- **必须**保留 Markdown 结构（标题、代码块、链接、表格）
- **必须**完整保留每轮 Agent 回复，禁止「详见上文」式省略
- **不省略**看似客套的提问（如「好的」「帮我把…」）——这些是用户的思路线索
- **不写入**工具调用日志、thinking 内容（UI 中不可见的部分）
- **仍脱敏**密码、Token、secret：结构保留，值替换为 `***`，文首注明「敏感信息已脱敏」

## summary 模式 CRITICAL 规则

- 按 [`conversation-template.md`](references/conversation-template.md) 整理，可摘要、合并、提炼结论
- 同样须脱敏敏感信息

## 通用 CRITICAL

- 归档的是「当前会话上下文」，不是编造内容
- 代码、API、路径以对话中实际出现的为准

## 文件名规则

| 模式 | 格式 | 示例 |
|------|------|------|
| verbatim | `YYYY-MM-DD-{主题}-实录.md` | `2026-07-02-工具编排型Skill写法-实录.md` |
| summary | `YYYY-MM-DD-{主题}.md` 或 `-摘要.md` | `2026-07-02-工具编排型Skill写法.md` |

| 规则 | 说明 |
|------|------|
| 日期 | 当天日期（用户时区，默认 GMT+8） |
| 主题 | 3～8 个汉字或英文词，概括对话核心 |
| 非法字符 | 去掉 `\ / : * ? " < > \|`，空格改 `-` |
| 长度 | 主题部分不超过 40 字符 |

## 触发话术

### verbatim（默认）

- 「帮我把上面对话存到 Obsidian」
- 「保存到 Obsidian」
- 「对话实录」
- 「原样保存这段对话」
- 「帮我把对话存到笔记」
- 「归档这段对话到知识库」

### summary（须明确）

- 「整理成摘要版存 Obsidian」
- 「整理成知识库版」
- 「格式化笔记存到 Obsidian」

用户若只说「整理成 md」但未说明要摘要还是实录，**默认 verbatim**；若语义模糊可简短确认。

## 能力边界

| 情况 | 说明 |
|------|------|
| 同一 session 内归档 | 可靠：Agent 能访问当前会话上下文 |
| 「一模一样」 | 指 **UI 可见的 Q&A 文本**一致，不是字节级 dump |
| 超长对话 | 若超出上下文窗口，最早几轮可能无法还原；能还原多少写多少，文首注明「可能不完整」 |
| 工具调用细节 | verbatim 模式默认不写入（curl 执行、Write 工具调用等） |

## 完成后回复格式

### verbatim

```markdown
已保存到 Obsidian（对话实录）：

- 路径：`D:\obsidian_file\pc_note\AI对话\xxx-实录.md`
- 轮次：N 轮（用户 N 条 / Assistant N 条）
- 说明：已按原文顺序保存；敏感信息已脱敏
```

### summary

```markdown
已保存到 Obsidian（知识库摘要版）：

- 路径：`D:\obsidian_file\pc_note\AI对话\xxx.md`
- 标题：xxx
- 摘要：一句话说明这篇笔记记录了什么
```

## 不在本 Skill 范围

- 修改已有 Obsidian 笔记内容 → 用户需指明文件路径
- 同步 Obsidian Sync / Git → 不负责，仅本地写文件
- 上传附件、图片 → 仅当对话中已有本地路径且用户明确要求时再处理
- 含工具调用日志的完整 debug 导出 → 暂无（可后续扩展）
