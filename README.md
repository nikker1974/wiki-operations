# Wiki Operations Skill / Wiki 操作技能

> **EN:** A Markdown knowledge base skill for AI Agents — dual-section pages + entity index, zero database required.  
> **ZH:** 给 AI Agent 用的 Markdown 知识库操作技能 — 双段式页面 + 实体索引，不需要数据库。

Inspired by **gbrain** (Garry Tan) and **Karpathy LLM Wiki**. Lightweight knowledge base designed for Hermes/OpenClaw Agents with native Chinese support. / 受 **gbrain** 和 **Karpathy LLM Wiki** 启发，专为 Hermes/OpenClaw Agent 设计的轻量级知识库方案，原生中文友好。

---

## Features / 特点

| English | 中文 |
|---------|------|
| 🚫 **No database** — Pure Markdown + Git, zero ops | 🚫 **不需要数据库** — 纯 Markdown + Git，零运维 |
| 🧠 **AI auto-maintains** — Agent updates index & logs when writing | 🧠 **AI 自动维护** — Agent 写内容时顺手更新索引和日志 |
| 📐 **Dual-section pages** — Mutable summary + immutable changelog | 📐 **双段式结构** — 可变摘要 + 不可变日志，保留决策痕迹 |
| 🔗 **Entity graph** — `entities.json` tracks people, companies, tools, concepts | 🔗 **实体关系图谱** — `entities.json` 自动追踪人、公司、工具、概念的关系 |
| 🇨🇳 **Chinese native** — No tokenization, regex, or plugin overhead | 🇨🇳 **中文原生友好** — 不用处理分词、正则匹配等问题 |

---

## Installation / 安装

### Hermes Agent

```bash
hermes skills install https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SKILL.md
```

Then tell your Agent: / 然后告诉 Agent：

> **EN:** "Read my wiki/SCHEMA.md and manage my knowledge base using the wiki-operations skill."  
> **ZH:** "读我的 wiki/SCHEMA.md，按 wiki-operations 技能管理知识库。"

### OpenClaw / Claude Code / Other Agents

Copy `SKILL.md` to a location your Agent can read, then reference it in system prompt or first conversation. / 复制 `SKILL.md` 到 Agent 可读取的位置，在系统提示词或首次对话中引用它。

---

## Quick Start / 快速开始

```bash
# 1. Initialize your knowledge base / 初始化知识库
mkdir my-wiki && cd my-wiki
git init

# 2. Create core files / 创建核心文件
curl -O https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SCHEMA.example.md
# Rename & edit SCHEMA.example.md → SCHEMA.md / 重命名并编辑

# 3. Create entities.json
echo '{"version":"1.0","updated":"'$(date +%F)'","people":{},"companies":{},"tools":{},"regulators":{},"concepts":{}}' > entities.json

# 4. Push to GitHub (or any Git remote)
git add -A && git commit -m "init: wiki initialized"
git remote add origin <your-repo-url>
git push -u origin main
```

---

## Core Concepts / 核心概念

### Dual-Section Pages / 双段式页面

Each page has two sections. / 每个页面分为两段：

```
## 当前理解 / Current Understanding（Mutable / 可变段）
Rewrite directly when understanding changes. / 更新时直接改写，始终是最新理解。

---
- 2026-05-04: Page created / 创建页面（Immutable / 不可变段，只追加不改）
```

| Section / 段落 | Editable? / 可改？ | Purpose / 用途 |
|----------------|-------------------|----------------|
| Top / 上层 | ✅ Rewrite anytime / 随时重写 | Current best understanding / 当前最准确的理解 |
| Bottom / 下层 | ❌ Append only / 只追加 | Edit trail, never modified / 编辑历史，永不修改 |

### Entity Index / 实体索引 (`entities.json`)

Auto-maintained by the Agent when creating/updating pages. Records typed relationships (`works_at`, `founded`, `regulated_by`, etc.) between people, companies, tools, and concepts. / Agent 在写页面时自动更新时间记录人物、公司、工具、概念之间的类型化关系。

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

---

## Recommended Directory Structure / 推荐目录结构

```
my-wiki/
├── SCHEMA.md          # Page conventions / 页面规范
├── index.md           # Page directory / 页面目录
├── log.md             # Operation log / 操作日志
├── entities.json      # Entity index (auto-maintained by Agent) / 实体索引（Agent自动维护）
├── personal/          # Your categories / 你的分类
│   ├── notes/
│   └── projects/
├── company/
│   ├── policies/
│   ├── legal/
│   └── benefits/
└── raw/               # Raw sources, read-only / 原始资料（只读）
    └── docs/
```

---

## Who Is This For? / 谁适合用这个？

**EN:**
- Users managing personal knowledge bases with Hermes/OpenClaw
- Those who find gbrain too heavy or poorly suited for Chinese content
- Anyone wanting a "low startup cost, scalable later" knowledge base
- Markdown note-takers who want an AI layer on top

**ZH:**
- 正在用 Hermes/OpenClaw 管理个人知识库的用户
- 觉得 gbrain 太重、对中文支持不够好的用户
- 想要一个"起步成本极低、将来可扩展"的知识库方案
- 习惯用 Markdown 做笔记、想给笔记加上 AI 层的人

---

## Related Projects / 相关项目

- [gbrain](https://github.com/garrytan/gbrain) — Garry Tan's AI Agent Brain (Postgres edition / Postgres 版)
- [hermes-agent](https://github.com/NousResearch/hermes-agent) — The Agent framework this skill targets / 本技能适配的 Agent 框架
- [OpenClaw](https://openclaw.ai/) — Another Agent framework / 另一个 Agent 框架
- [hermes-wiki](https://github.com/nikker1974/hermes-wiki) — Real-world wiki running this skill / 本技能真实运行的 wiki 示例

---

## License / 许可证

MIT
