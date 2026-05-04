# Wiki Operations Skill

> A Markdown knowledge base skill for AI Agents — dual-section pages + entity index, zero database required.

Inspired by **gbrain** (Garry Tan) and **Karpathy LLM Wiki**. Lightweight knowledge base designed for Hermes/OpenClaw Agents with native Chinese support.

## Features

- 🚫 **No database** — Pure Markdown + Git, zero ops
- 🧠 **AI auto-maintains** — Agent updates index & logs when writing
- 📐 **Dual-section pages** — Mutable summary + immutable changelog
- 🔗 **Entity graph** — `entities.json` tracks people, companies, tools, concepts relationships
- 🇨🇳 **Chinese native** — No tokenization, regex, or plugin overhead

## Installation

### Hermes Agent

```bash
hermes skills install https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SKILL.md
```

Then tell your Agent:

> "Read my wiki/SCHEMA.md and manage my knowledge base using the wiki-operations skill."

### OpenClaw / Claude Code / Other Agents

Copy `SKILL.md` to a location your Agent can read, then reference it in system prompt or first conversation.

## Quick Start

```bash
# 1. Initialize your knowledge base
mkdir my-wiki && cd my-wiki
git init

# 2. Create core files
#   - SCHEMA.md (page conventions, customize to your needs)
#   - index.md (page directory)
#   - log.md (operation log)
#   - entities.json (entity index)

# 3. Initialize entities.json
echo '{"version":"1.0","updated":"'$(date +%F)'","people":{},"companies":{},"tools":{},"regulators":{},"concepts":{}}' > entities.json

# 4. Push to GitHub (or any Git remote)
git add -A && git commit -m "init: wiki initialized"
git remote add origin <your-repo-url>
git push -u origin main
```

## Core Concepts

### Dual-Section Pages

Each page has two sections:

```
---
title: Example Page
created: 2026-05-04
updated: 2026-05-04
type: concept
tags: [example]
---

## Current Understanding (Mutable)

Rewrite directly when understanding changes. Use bullet points or tables for scannability.

---
- 2026-05-04: Page created (Immutable — append only, never modify)
```

| Section | Behavior | Purpose |
|---------|----------|---------|
| Top (above `---`) | Rewrite anytime | Current best understanding |
| Bottom (below `---`) | Append only, never modify | Edit trail, decision history |

### Entity Index (`entities.json`)

Auto-maintained by the Agent when creating or updating pages. Records typed relationships between entities.

```json
{
  "people": {
    "ALICE": {
      "aliases": ["Alice Chen"],
      "page": "team/alice",
      "relations": [
        {"type": "works_at", "target": "ACME_CORP", "detail": "Software Engineer"}
      ]
    }
  }
}
```

Entity types: `people`, `companies`, `tools`, `regulators`, `concepts`.  
Relationship types: `works_at`, `founded`, `ceo_of`, `regulated_by`, `created`, `used_by`, `insures`, etc.

## Recommended Directory Structure

```
my-wiki/
├── SCHEMA.md          # Page conventions
├── index.md           # Page directory
├── log.md             # Operation log
├── entities.json      # Entity index (auto-maintained by Agent)
├── personal/
│   ├── notes/
│   └── projects/
├── company/
│   ├── policies/
│   ├── legal/
│   └── benefits/
└── raw/               # Raw source files, read-only
    └── docs/
```

## Who Is This For?

- Users managing personal knowledge bases with Hermes/OpenClaw
- Those who find gbrain too heavy or poorly suited for Chinese content
- Anyone wanting a "low startup cost, scalable later" knowledge base
- Markdown note-takers who want an AI layer on top

## Related Projects

- [gbrain](https://github.com/garrytan/gbrain) — Garry Tan's AI Agent Brain (Postgres edition)
- [hermes-agent](https://github.com/NousResearch/hermes-agent) — The Agent framework this skill targets
- [OpenClaw](https://openclaw.ai/) — Another Agent framework
- [hermes-wiki](https://github.com/nikker1974/hermes-wiki) — Real-world wiki running this skill

## License

MIT

---

<br>

---

# Wiki 操作技能

> 给 AI Agent 用的 Markdown 知识库操作技能 — 双段式页面 + 实体索引，不需要数据库。

受 **gbrain**（Garry Tan）和 **Karpathy LLM Wiki** 启发，专为 Hermes/OpenClaw Agent 设计的轻量级知识库方案，原生中文友好。

## 特点

- 🚫 **不需要数据库** — 纯 Markdown + Git，零运维
- 🧠 **AI 自动维护** — Agent 写内容时顺手更新索引和日志
- 📐 **双段式结构** — 可变摘要 + 不可变日志，保留决策痕迹
- 🔗 **实体关系图谱** — `entities.json` 自动追踪人、公司、工具、概念的关系
- 🇨🇳 **中文原生友好** — 不用处理分词、正则匹配等问题

## 安装

### Hermes Agent

```bash
hermes skills install https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SKILL.md
```

然后告诉 Agent：

> "读我的 wiki/SCHEMA.md，按 wiki-operations 技能管理知识库。"

### OpenClaw / Claude Code / 其他 Agent

复制 `SKILL.md` 到 Agent 可读取的位置，在系统提示词或首次对话中引用它。

## 快速开始

```bash
# 1. 初始化知识库
mkdir my-wiki && cd my-wiki
git init

# 2. 创建核心文件
#   - SCHEMA.md（页面规范，按需定制）
#   - index.md（页面目录）
#   - log.md（操作日志）
#   - entities.json（实体索引）

# 3. 初始化 entities.json
echo '{"version":"1.0","updated":"'$(date +%F)'","people":{},"companies":{},"tools":{},"regulators":{},"concepts":{}}' > entities.json

# 4. 推送到 GitHub（或其他 Git 仓库）
git add -A && git commit -m "init: wiki initialized"
git remote add origin <你的仓库地址>
git push -u origin main
```

## 核心概念

### 双段式页面

每个页面分为两段：

```
---
title: 示例页面
created: 2026-05-04
updated: 2026-05-04
type: concept
tags: [示例]
---

## 当前理解（可变段）

更新时直接改写。建议用要点或表格呈现，方便快速浏览。

---
- 2026-05-04: 创建页面（不可变段，只追加不改）
```

| 段落 | 行为 | 用途 |
|------|------|------|
| 上层（分隔线上方） | 随时重写 | 当前最准确的理解 |
| 下层（分隔线下方） | 只追加，永不修改 | 编辑历史、决策痕迹 |

### 实体索引（entities.json）

Agent 在创建或更新页面时自动维护。记录实体之间的类型化关系。

```json
{
  "people": {
    "张三": {
      "aliases": ["张三丰"],
      "page": "团队/张三",
      "relations": [
        {"type": "works_at", "target": "某公司", "detail": "软件工程师"}
      ]
    }
  }
}
```

实体分类：`people`（人）、`companies`（公司）、`tools`（工具）、`regulators`（监管机构）、`concepts`（概念）。  
关系类型：`works_at`、`founded`、`ceo_of`、`regulated_by`、`created`、`used_by`、`insures` 等。

## 推荐目录结构

```
my-wiki/
├── SCHEMA.md          # 页面规范
├── index.md           # 页面目录
├── log.md             # 操作日志
├── entities.json      # 实体索引（Agent 自动维护）
├── 个人/
│   ├── 笔记/
│   └── 项目/
├── 公司/
│   ├── policies/
│   ├── legal/
│   └── benefits/
└── raw/               # 原始资料（只读）
    └── docs/
```

## 谁适合用这个？

- 正在用 Hermes/OpenClaw 管理个人知识库的用户
- 觉得 gbrain 太重、对中文支持不够好的用户
- 想要一个"起步成本极低、将来可扩展"的知识库方案
- 习惯用 Markdown 做笔记、想给笔记加上 AI 层的人

## 相关项目

- [gbrain](https://github.com/garrytan/gbrain) — Garry Tan 的 AI Agent Brain（Postgres 版）
- [hermes-agent](https://github.com/NousResearch/hermes-agent) — 本技能适配的 Agent 框架
- [OpenClaw](https://openclaw.ai/) — 另一个 Agent 框架
- [hermes-wiki](https://github.com/nikker1974/hermes-wiki) — 本技能真实运行的 wiki 示例

## 许可证

MIT
