[![ClawHub](https://img.shields.io/badge/ClawHub-v3.0.5-blue)](https://clawhub.ai/package/local-knowledge-retrieval)

# knowledge-retrieval — 本地知识库检索 Skill

> 给知识工作者和顾问的本地文件检索方案
> 适合：手里有大量 PPT/PDF 本地文档、想用 AI 搜索但不等于搬上云的人

---

## 它是给谁用的

如果你和我一样：

- 手上积累了 **10 年以上的工作文档**，大部分在本地硬盘里，不是在线文档
- 文件格式**主要是 PPT、PDF、扫描件、图片**，而不是干净的 Markdown
- 想把 AI 的能力用到这些老材料上，但 **不想搬上云、不想转格式、不想折腾向量数据库**
- 你习惯带着**明确的关键词或者标签**去搜索，但有时候也想像聊天一样「帮我找一下和某某有关的那份报告」

那你大概率会被这个工具吸引。

---

## 它能解决什么问题

### 🔑 你的文档格式，它都读得懂

市面上多数搜索方案要求纯文本。我们的方案能直接处理这些格式：

| 格式 | 支持情况 |
|------|---------|
| PPTX | 全文提取（含嵌套图形、备注页） |
| PDF | 双引擎兜底（pdfminer → PyMuPDF） |
| DOCX / XLSX | 全文提取 |
| TXT / MD / CSV | 当然支持 |
| 图片 | 嵌入文字识别 |

文件原地读取，原文件夹不受任何影响。非纯文本文件（PDF、PPTX、DOCX）的文字提取缓存存放在独立的 skill 工作目录中，不和原文件夹混在一起。

### 🔑 本地优先

你的文档在自己电脑上，不用上传到任何云端。知识库索引也建在本地。

这对很多顾问来说不是偏好问题，是**合规底线**——客户材料不能上传第三方服务。

**也支持网盘文件。** 如果文件存放在 OneDrive 等本地同步盘里，建库和搜索时系统会自动从云端下载。首次建索引需要联网。

### 🔑 动态更新，改了就能搜到

你的知识库不是静态的——每天都在加新材料、改旧报告。索引支持增量更新：

- 新文件加入 → 下次搜索自动发现
- 旧文件修改 → 描述自动刷新
- 不需要每次改文件都跑一次完整重建

大多数方案初始化即定型。我们是伴随使用持续进化的。

### 🔑 精准关键词 + 自然语言，两条路都通

纯关键词搜索的痛点：同一种概念在不同文件里措辞不同（搜「TRL」找不到标题为「技术成熟度评估」的文件）。纯语义搜索的痛点：模糊查询容易跑偏，而且依赖向量库维护。

我们的方式：**AI 先帮你扩展关键词，再交给轻量关键词索引精确命中。** 双通道输出合并排序——你不需要记住文件里用的具体是什么词，只要概念是对的就能找到。

### 🔑 越用越聪明

传统方案：初始化跑 30 分钟建好索引 → 之后搜索质量固定了。

我们：**第一次搜索时只建最基础的索引。** 每次你搜到一个文件，AI 读完内容后会顺手更新它的描述。随着使用：

- 最常被搜到的文件 → 描述最丰富 → 搜索命中率自然最高
- 不常被搜到的文件 → 不浪费预处理时间
- 你的搜索习惯 → 逐渐塑造出对你最友好的索引

**伴随使用持续进化。**

### 🔑 配置不全也能跑

假设你电脑上没有 Python PDF 库——没关系，搜索仍能运行，只是 PDF 文件暂时搜不到。
假设 BM25 的索引丢了——没关系，自动降级到纯 AI 语义匹配，不会卡住。
假设知识库里一个匹配文件都没有——AI 不会编造一个答案，它会诚实告诉你没找到。

---

## 和同类方案的快速对比

| 维度 | file-search | semfind | 我们 |
|------|-----------|---------|------|
| PPT/PDF 支持 | ❌ | ❌ | ✅ 完整提取 |
| 本地优先 | ✅ | ✅ | ✅ |
| 动态更新 | ✅ | ❌ 一次性索引 | ✅ 增量感知 |
| 搜索方式 | 纯关键词 | 纯语义 | 关键词 + 语义双通道 |
| 越用越聪明 | ❌ | ❌ | ✅ 描述进化 |
| 降级能力 | 报错卡住 | 报错卡住 | ✅ 每步有兜底 |
| 初始化时间 | 秒级 | 分钟-小时 | 秒级（渐进式） |

---

## 快速开始

```bash
openclaw skills install local-knowledge-retrieval
```

之后 Agent 会自动检测你的知识库文件夹，不需要手动配置。

## 环境要求

```bash
pip install bm25s pdfminer.six python-pptx
# 可选：python-docx（DOCX 支持）
```

## 系统兼容

- **Windows：** 全功能可用（含 OneDrive 云端文件自动下载）
- **macOS / Linux：** 核心搜索功能完整可用。OneDrive 文件自动下载依赖 Windows 系统特性
- **Shell 命令：** 以 Windows 为主（`dir /b` / `Select-String`），Mac 可用 `ls` / `grep` 替代

---

## ⚠️ 免责声明

> 本项目由咨询顾问基于实际工作流开发，主打解决真实业务痛点，非企业级商业软件。
> 业余时间维护（Best Effort），欢迎提 PR，但不保证及时回复所有 Issue。

## License

MIT — do what you want, no strings attached.

---

*Built with 🐾 by Zara & 小爪*

---

# knowledge-retrieval — Local Knowledge Base Search Skill

> A local document search skill designed for knowledge workers and consultants.
> For people who have accumulated years of PPTs, PDFs, and reports on their local drive and want AI-powered search — without moving anything to the cloud.

---

## Who this is for

You, if:

- You have **10+ years of work documents** sitting on your local hard drive
- Your files are **mostly PPTs, PDFs, scanned documents, and images**
- You want AI to make these materials searchable, but you **don't want to upload, convert formats, or set up a vector database**
- You search with precise keywords, but sometimes want to ask naturally

## What it solves

### Reads your actual file formats

| Format | Support |
|--------|---------|
| PPTX | Full text extraction (nested shapes, speaker notes) |
| PDF | Dual-engine fallback (pdfminer → PyMuPDF) |
| DOCX / XLSX | Full text extraction |
| TXT / MD / CSV | Yes |
| Images | Embedded text recognition |

### Local-first

Your documents stay on your machine. The index is built locally. Nothing is uploaded anywhere.

### Dynamic updates

New files are discovered on the next search. Modified files auto-refresh. No need to rebuild after every change.

### Dual-channel search

AI expands your query into synonyms, then hands it to a lightweight keyword index for precision matching. You don't need to guess what words the author used.

### Gets smarter with use

Only the minimal index is built on first use. Every search improves it. Most-searched files get the richest descriptions.

### Graceful degradation

No PDF library? Search still runs, just skips PDFs. BM25 corrupted? Falls back to AI semantic matching.

## Quick start

```bash
openclaw skills install local-knowledge-retrieval
```

## Requirements

```bash
pip install bm25s pdfminer.six python-pptx
```

---

> **Disclaimer:** This project is developed by a consultant, for consultants. Maintained on a best-effort basis. PRs welcome.
