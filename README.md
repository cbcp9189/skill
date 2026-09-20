# Codex Skills

这个仓库收集个人使用的 Codex Skills。每个 Skill 都放在独立目录中，以 `SKILL.md` 定义适用场景和执行流程，`references/` 存放模板或补充说明。

## 目录

| Skill | 用途 | 入口 |
| --- | --- | --- |
| Obsidian 对话归档 | 将当前对话保存为 Markdown 实录，或整理为知识库摘要 | [obsidian-chat-archive/SKILL.md](obsidian-chat-archive/SKILL.md) |
| GitBook API 发布 | 按白名单检查 EnjoyIot SaaS API 变更、创建 GitBook 草稿，并在明确指定草稿后发布 | [gitbook-api-publisher/SKILL.md](gitbook-api-publisher/SKILL.md) |

```text
.
├── obsidian-chat-archive/
│   ├── SKILL.md
│   └── references/
│       ├── conversation-template.md
│       └── conversation-transcript-template.md
└── gitbook-api-publisher/
    ├── SKILL.md
    ├── agents/openai.yaml
    └── references/
        ├── ebelong-gitbook.md
        ├── manifest.md
        └── publish-manifest.template.yaml
```

## 安装与使用

将需要的 Skill 目录复制到 Codex 的用户级 Skill 目录 `~/.agents/skills/`。在 Windows PowerShell 中，可以这样安装：

```powershell
git clone https://github.com/cbcp9189/skill.git
cd skill
New-Item -ItemType Directory -Force "$HOME\.agents\skills" | Out-Null
Copy-Item -Recurse -Force .\obsidian-chat-archive "$HOME\.agents\skills\"
Copy-Item -Recurse -Force .\gitbook-api-publisher "$HOME\.agents\skills\"
```

按需复制其中一个目录即可。安装后，可以在 Codex 对话中直接描述任务，或用 `$obsidian-chat-archive`、`$gitbook-api-publisher` 显式指定 Skill。若新安装的 Skill 没有出现，重启 Codex。

### Obsidian 对话归档

默认将**当前对话**按轮次保存为实录。只有明确要求“摘要版”“知识库版”或“格式化笔记”时，才会使用摘要模板。归档时会脱敏密码、Token 等敏感信息；实录不包含工具调用日志。

示例：

```text
把这段对话原样保存到 Obsidian。
把这段对话整理成知识库摘要版，保存到 Obsidian。
把这段对话保存到 Obsidian 的 10-Projects/项目名 目录。
```

使用前，请在 [SKILL.md](obsidian-chat-archive/SKILL.md) 的“路径配置”中，将默认 Obsidian 仓库路径 `D:\obsidian_file\pc_note` 改成自己的实际路径。默认子目录是 `AI对话`。两个模板沿用了原有的 `cursor` 标签和 `source: cursor-agent` 字段；如需标记为 Codex，可自行修改 `references/` 中对应模板。

该 Skill 只根据当前会话中可访问的内容归档。若要保存一条旧对话，建议先打开那条对话再发出归档指令；超长对话的早期内容可能无法完整还原。

### GitBook API 发布

此 Skill 面向 EnjoyIot SaaS API 文档。它以 API 项目中的 `doc/gitbook-api-publish.yaml` 为发布白名单，按明确的 `operationId` 选择接口；仓库中的 [清单模板](gitbook-api-publisher/references/publish-manifest.template.yaml) 和 [配置说明](gitbook-api-publisher/references/manifest.md) 可用于初始化和维护白名单。

示例：

```text
检查已允许发布的 SaaS API 有哪些变更。
同步已允许发布的接口，并创建 GitBook 草稿。
发布 GitBook 草稿 6。
```

“检查”只报告变化；“同步”创建或更新草稿。发布需要明确指出要发布的草稿。具体目标站点与页面规则见 [GitBook 配置说明](gitbook-api-publisher/references/ebelong-gitbook.md)。

## 说明

本仓库保存 Skill 指令、模板和配置示例，不包含 Obsidian 笔记或 API 项目的 OpenAPI 文档。使用各 Skill 前，请阅读其 `SKILL.md` 中的适用范围与前置配置。
