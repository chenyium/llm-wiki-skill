# Schema 模板

初始化 wiki 项目时，根据用户主题调整 `{{占位符}}` 后写入 Schema 文件。

Schema 文件名随平台而定：`CLAUDE.md`（Claude Code）、`AGENTS.md`（Codex/OpenCode）、`.cursorrules`（Cursor）等。

---

```markdown
# {{WIKI_TITLE}}

这是一个由 LLM 维护的个人知识库，主题：{{TOPIC_DESCRIPTION}}。

用户负责添加原始资料和提问，LLM 负责所有整理、交叉引用和维护工作。
Schema 由用户和 LLM 共同演进——发现新的有效模式时，更新此文件。

## 目录结构

- `raw/` — 原始资料，**只读**。LLM 不修改此目录。
  - `assets/` — 图片等附件（可选，纯文本项目可省略）。
- `wiki/` — LLM 维护的知识库。
  - `index.md` — 内容目录，按分类组织，每次摄入后更新。
  - `log.md` — 操作日志，append-only，按时间顺序。
  - `entities/` — 实体页（人物、组织、工具、项目等）。
  - `concepts/` — 概念页（理论、方法论、模式等）。
  - `sources/` — 每篇原始资料的摘要页。
  - `analyses/` — 查询产出的分析、对比、综合页。

以上分类为默认建议，可根据领域裁剪（增删目录、合并类别等）。

## 页面约定

### 文件名

小写字母、数字、连字符。例：`transformer-architecture.md`、`openai.md`。

### Frontmatter

每个 wiki 页面必须包含：

```yaml
---
title: 页面标题
tags: [相关标签]
sources: [来源资料的 slug 列表]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Dataview 等 Obsidian 插件可基于 frontmatter 生成动态表格和统计。

### 交叉引用

使用 `[[文件名]]` 格式（不含 `.md` 后缀），与 Obsidian 兼容。

### 内容质量

- 陈述事实时标注来源：`（来源：[[source-slug]]）`
- 存在矛盾时明确标记：`⚠️ 与 [[other-page]] 的说法冲突`
- 不确定的内容标记：`❓ 待验证`

## 操作流程

### Ingest（摄入新资料）

1. 用户指定 `raw/` 中的文件
2. 完整阅读资料
3. 与用户讨论关键发现
4. 创建 `wiki/sources/<slug>.md`（摘要 + 要点）
5. 创建或更新相关实体页和概念页（注意标记与已有内容的矛盾）
6. 更新 `wiki/index.md`
7. 追加 `wiki/log.md`

建议逐篇摄入并保持参与；也可批量摄入减少监督。
一次摄入可能触及 10-15 个页面。

### Query（查询）

1. 先读 `wiki/index.md` 定位相关页面
2. 读取相关页面，综合回答，引用具体页面
3. 回答形式灵活——Markdown 页面、对比表格、Marp 幻灯片、matplotlib 图表、canvas 均可
4. **有长期参考价值的回答，归档为 `wiki/analyses/<slug>.md` 并更新 index**

让探索和分析也沉淀为知识，而非消失在聊天记录里。

### Lint（巡检）

检查以下问题并生成报告：

- 页面间矛盾
- 被新资料取代的旧内容（stale claims）
- 无入站链接的孤立页面
- 被多次提及但没有独立页面的概念
- 缺失的交叉引用
- 值得补充的数据缺口

巡检后**主动建议**：
- 值得继续探究的新问题
- 值得寻找的新资料来源

## index.md 格式

```markdown
# {{WIKI_TITLE}} — 目录

## 实体
- [[entity-slug]] — 一句话描述

## 概念
- [[concept-slug]] — 一句话描述

## 资料摘要
- [[source-slug]] — 来源类型 | 一句话描述

## 分析
- [[analysis-slug]] — 一句话描述
```

在中等规模（~100 篇资料、数百页面）下，index 足以定位内容。
规模更大时引入搜索工具（如 [qmd](https://github.com/tobi/qmd)）。

## log.md 格式

每条日志以 `## [YYYY-MM-DD] 操作类型 | 标题` 开头，便于 unix 工具解析：
`grep "^## \[" log.md | tail -5` 可快速查看最近 5 条操作。

```markdown
# 操作日志

## [YYYY-MM-DD] ingest | 资料标题
- 创建：列出新建的页面
- 更新：列出更新的页面
- 要点：一句话核心发现

## [YYYY-MM-DD] query | 问题概要
- 回答要点
- 归档：analyses/slug.md（如有）

## [YYYY-MM-DD] lint
- 发现 N 个问题
- 已修复：列出
- 待处理：列出
- 建议：值得探究的问题、值得补充的资料
```

## 图片处理（可选）

如果资料包含图片：

1. 在 Obsidian 设置 → Files and links 中，设置附件目录为 `raw/assets/`
2. 在 Hotkeys 中绑定"Download attachments"快捷键（如 Ctrl+Shift+D）
3. 裁剪文章后按快捷键，图片自动下载到本地

LLM 无法一次性阅读含内嵌图片的 Markdown。变通方式：先读文本，再单独查看引用的图片获取额外上下文。
```
