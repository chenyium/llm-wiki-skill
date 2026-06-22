# LLM Wiki Skill

基于 [Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)，让 LLM 持续构建和维护结构化个人知识库。

**核心理念**：知识编译一次、持续更新，而非每次查询时重新推导。你负责喂资料和提问，LLM 负责所有整理工作。

## 功能

- **Ingest** — 摄入新资料，自动生成摘要、实体、概念页面并建立交叉引用
- **Query** — 基于 wiki 回答问题，有价值的回答自动归档
- **Lint** — 巡检 wiki 健康度（矛盾、孤立页、缺失引用等）

## 安装

### 方式一：npx skills（推荐）

```bash
npx skills add chenyium/llm-wiki-skill
```

### 方式二：手动软链接

```bash
git clone https://github.com/chenyium/llm-wiki-skill.git
ln -s /path/to/llm-wiki-skill/llm-wiki ~/.agents/skills/llm-wiki
```

## 使用

```
/llm-wiki
```

在对话中输入 `/llm-wiki` 即可调用。支持所有兼容 Agent Skills 的平台（Cursor、Claude Code、Codex、OpenCode 等）。

### 典型使用流程

```
你：「帮我初始化一个 AI 论文研究 wiki，放在 ~/wikis/ai-research」
我：创建目录、Schema、首次提交

你：把论文 PDF 或 Markdown 放进 raw/，说「消化 raw/attention-is-all-you-need.md」
我：生成摘要、创建实体页（Transformer、Google Brain）、概念页（Self-Attention）

你：「Transformer 和 RNN 的核心区别是什么？」
我：综合已有页面回答，并归档为 analyses/transformer-vs-rnn.md

你：「巡检一下」
我：报告问题、建议补充方向
```

## 适用场景

- 个人成长（目标、健康、心理）
- 深度研究（论文、报告）
- 读书伴侣（逐章摄入、角色/主题追踪）
- 团队知识库（Slack、会议记录、项目文档）
- 竞品分析、尽职调查、课程笔记等

## 工具链搭配

| 工具 | 用途 |
|------|------|
| Obsidian | 浏览 wiki、图谱视图 |
| Obsidian Web Clipper | 一键裁剪网页 |
| Marp | 从 wiki 生成幻灯片 |
| Dataview | frontmatter 动态查询 |
| Git | 版本管理 |

## License

[MIT](LICENSE)
