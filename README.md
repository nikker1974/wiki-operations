# Wiki Operations 技能

> 让 AI Agent 拥有长期记忆的 Markdown 知识库操作方案

受 **gbrain** (Garry Tan) 和 **Karpathy LLM Wiki** 启发，专为中文用户和 Hermes/OpenClaw Agent 设计的轻量级知识库方案。

## 特点

- 🚫 **不需要数据库** — 纯 Markdown + Git，零运维
- 🧠 **AI 自动维护** — Agent 写内容时顺手更新索引和日志
- 📐 **双段式结构** — 可变摘要 + 不可变日志，保留决策痕迹
- 🔗 **实体关系图谱** — entities.json 自动追踪人、公司、工具、概念的关系
- 🇨🇳 **中文原生友好** — 不用处理中文分词、正则匹配等问题

## 安装

### Hermes Agent

```bash
hermes skills install https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SKILL.md
```

然后启动 Agent：

> "读我的 wiki/SCHEMA.md，按 wiki-operations 技能管理知识库"

### OpenClaw / Claude Code / 其他 Agent

复制 `SKILL.md` 到你的 Agent 可以读取的位置，然后在系统提示词或首次对话中引用它。

## 快速开始

```bash
# 1. 初始化你的知识库
mkdir my-wiki && cd my-wiki
git init

# 2. 创建核心文件
curl -O https://raw.githubusercontent.com/nikker1974/wiki-operations/main/SCHEMA.example.md
# 重命名并编辑 SCHEMA.example.md → SCHEMA.md

# 3. 创建 entities.json
echo '{"version":"1.0","updated":"'$(date +%F)'","people":{},"companies":{},"tools":{},"regulators":{},"concepts":{}}' > entities.json

# 4. 推送到 GitHub（或其他 Git 仓库）
git add -A && git commit -m "init: wiki initialized"
git remote add origin <your-repo-url>
git push -u origin main
```

## 核心概念

### 双段式页面

每个页面分为两段：

```
## 当前理解（可变段）
更新时直接改写，始终是最新理解

---
- 2026-05-04: 创建页面（不可变段，只追加不改）
```

### entities.json 实体索引

自动记录人物、公司、工具、概念之间的类型化关系（`works_at`、`founded`、`regulated_by` 等），由 Agent 在写页面时同步更新。

## 目录结构示例

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
│   └── legal/
└── raw/               # 原始资料（只读）
```

## 谁适合用这个？

- 正在用 Hermes / OpenClaw 管理个人知识库的用户
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
