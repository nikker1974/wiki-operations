---
name: wiki-operations
description: "操作 Markdown wiki 知识库。双段式页面格式 + entities.json 实体索引 + 日志维护，让 AI Agent 拥有长期记忆。"
version: 1.0.0
author: Nikker (nikker1974)
tags: [wiki, knowledge-base, markdown, gbrain-style, dual-section, memory]
---

# Wiki Operations

一个给 AI Agent（Hermes、OpenClaw、Claude Code 等）用的 **Markdown 知识库操作规范**。让 Agent 能够：

- 用统一的**双段式格式**读写 wiki 页面
- 自动维护 **entities.json 实体索引**（人物、公司、工具、概念的关系图谱）
- 记录每次操作的**编辑日志**

## 架构理念

受 gbrain（Garry Tan）和 Karpathy LLM-wiki 启发，但做了更适合中文用户的简化：

- **纯 Markdown + Git** — 不需要数据库，不需要服务器
- **AI 维护** — Agent 写内容时顺手维护索引和日志
- **双段式** — 每个页面分"可变摘要"和"不可变日志"两部分

## 快速开始

### 1. 建立你的 wiki

```bash
git init my-wiki
cd my-wiki
```

### 2. 下载本技能

```bash
# Hermes Agent
hermes skills install https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SKILL.md

# 或手动复制 SKILL.md 到 ~/.hermes/skills/note-taking/wiki-operations/
```

### 3. 创建 SCHEMA.md

```markdown
# Wiki Schema

## Domain
你的知识库主题（个人笔记 / 公司文档 / 研究资料）

## Conventions
- 文件命名：全小写、连字符
- 每页以 YAML frontmatter 开头
- 更新时同步更新 `updated` 日期
- 新建页面加入 index.md

## Frontmatter
---
title: 页面标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | summary
tags: [...]
sources: [raw/...]
confidence: high | medium | low
---
```

### 4. 创建 entities.json

```json
{
  "version": "1.0",
  "updated": "2026-05-04",
  "people": {},
  "companies": {},
  "tools": {},
  "regulators": {},
  "concepts": {}
}
```

### 5. 告诉你的 Agent

启动 Agent 后说：

> "读 ~/wiki/SCHEMA.md，然后按 wiki-operations 技能管理我的 wiki"

## 页面格式：双段式

每个页面使用两种分界明确的段落：

```
---
title: 示例页面
created: 2026-05-04
updated: 2026-05-04
type: concept
tags: [示例]
confidence: high
---

## 当前理解（可变段）

当前最准确的摘要。内容更新时直接改写本段。
要点式、表格化呈现，不堆砌冗余细节。

---
- 2026-05-04: 创建页面
- 2026-05-04: 追加日志，永不修改（不可变段）
```

### 可变段
- 存放当前最准确的理解
- 有新理解时**直接重写**
- 建议表格、要点式呈现，方便快速扫描

### 不可变段
- 每次操作追加一行
- **永不删除、永不修改**
- 格式：`- YYYY-MM-DD: 做了什么`

## 实体索引：entities.json

每次写页面时自动维护此文件。

### 分类

| 分类 | 键名 | 内容 |
|------|------|------|
| 人物 | `people` | 团队成员、客户、合作者、公众人物 |
| 公司 | `companies` | 雇主、客户公司、合作伙伴 |
| 工具 | `tools` | 软件、框架、服务 |
| 监管 | `regulators` | 监管机构、政府部门 |
| 概念 | `concepts` | 理论、方法、术语 |

### 实体格式

```json
{
  "PERSON_ID": {
    "name": "显示名（可选）",
    "aliases": ["别名1", "别名2"],
    "page": "wiki/相对路径/不加后缀",
    "relations": [
      {"type": "works_at", "target": "COMPANY_ID", "detail": "职位描述"}
    ]
  }
}
```

### 规则
- 实体 ID 用大写字母 + 下划线（如 `ZO_ASSET_MANAGEMENT`）
- `page` 字段使用 wiki 内的相对路径，**去掉 `.md` 后缀**
- 发现新实体 → 添加，发现新关系 → 追加，发现新别名 → 追加
- 关系类型示例：`works_at`, `founded`, `ceo_of`, `regulated_by`, `created`, `used_by`, `insures`

## 更新日志

每次操作后追加到 `log.md`：

```markdown
## [YYYY-MM-DD] action | 一句话摘要
- 做了什么、改了什么、新增了什么
```

action 类型：`create | update | ingest | refactor | query | lint`

## 冲突处理

新信息与旧内容冲突时：
1. 比较日期 — 更新的一般为准
2. 真正矛盾的 → 标注两种观点，记录日期和来源
3. 在 frontmatter 标记 `confidence: medium`

## 建议的目录结构

```
my-wiki/
├── SCHEMA.md          # 规范文件（定制你的规则）
├── index.md           # 页面目录
├── log.md             # 操作日志
├── entities.json      # 实体索引
├── 个人/              # 你的分类
│   ├── 笔记/
│   └── 项目/
├── 公司/
│   ├── policies/
│   ├── legal/
│   └── benefits/
└── raw/               # 原始资料（只读）
    └── docs/
```

## 关于

本技能受以下项目启发：
- **gbrain** by Garry Tan (YC CEO) — [GitHub](https://github.com/garrytan/gbrain)
- **LLM Wiki** by Andrej Karpathy — Karpathy 提出的知识库概念
- **hermes-wiki** by Nikker — 中文环境下实际运行的生产知识库
