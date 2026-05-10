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

文件原地读取，原文件夹不受任何影响。知识库仅建立**双向快捷方式**指向原始文件。非纯文本文件（PDF、PPTX、DOCX）的文字提取缓存存放在独立的 skill 工作目录中，不和原文件夹混在一起。

### 🔑 本地优先

你的文档在自己电脑上，不用上传到任何云端。知识库索引也建在本地。

这对很多顾问来说不是偏好问题，是**合规底线**——客户材料不能上传第三方服务。

**也支持网盘文件。** 如果文件存放在 OneDrive 等本地同步盘里，建库和搜索时系统会自动从云端下载。首次建索引需要联网。

### 🔑 动态更新，改了就能搜到

你的知识库不是静态的——每天都在加新材料、改旧报告。我们的索引支持增量更新：

- 新文件加入 → 下次搜索自动发现
- 旧文件修改 → 描述自动刷新
- 不需要每次改文件都跑一次完整重建

大多数方案初始化即定型。我们是伴随使用持续进化的。

### 🔑 精准关键词 + 自然语言，两条路都通

纯关键词搜索的痛点：同一种概念在不同文件里措辞不同（搜「TRL」找不到标题为「技术成熟度评估」的文件）。纯语义搜索的痛点：模糊查询容易跑偏，而且依赖向量库维护。

我们的方式：**AI 先帮你扩展关键词，再交给轻量关键词索引精确命中。**

```
你问：「帮我找一下氢能产业链分析的报告」
     ↓
AI 扩展同义词（≤ 20 个）:
  「氢能」→ 氢能、氢气、氢能源、hydrogen energy
  「产业链」→ 产业链、供应链、价值链、产业生态
  「分析」→ 分析、评估、研究、报告、白皮书
     ↓
关键词索引带着这组扩展词去全文搜索
     ↓
AI 语义通道再做一次判断兜底
     ↓
双通道合并排序
```

这套组合意味着：你不需要记住文件里用的具体是什么词。只要概念是对的，AI 和关键词引擎会联手帮你找到它。

### 🔑 越用越聪明

传统方案：初始化跑 30 分钟建好索引 → 之后搜索质量固定了。

我们：**第一次搜索时只建最基础的索引**。每次你搜到一个文件，AI 读完内容后会顺手更新它的描述。随着使用：

- 最常被搜到的文件 → 描述最丰富 → 搜索命中率自然最高
- 不常被搜到的文件 → 不浪费预处理时间
- 你的搜索习惯 → 逐渐塑造出对你最友好的索引

**伴随使用持续进化。**

### 🔑 配置不全也能跑

假设你电脑上没有 Python PDF 库——没关系，搜索仍能运行，只是 PDF 文件暂时搜不到。
假设 BM25 的索引丢了——没关系，自动降级到纯 AI 语义匹配，不会卡住。
假设知识库里一个匹配文件都没有——AI 不会编造一个答案，它会诚实告诉你没找到。

你不需要为工具的不完美焦虑，它自己会扛。

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
# 安装
openclaw skills install knowledge-retrieval

# 之后 Agent 会自动检测你的知识库文件夹，不需要手动配置
```

## 环境要求

```bash
pip install bm25s pdfminer.six python-pptx
# 可选：python-docx（DOCX 支持）
```

## 系统兼容

- **Windows：** 全功能可用（含 OneDrive 云端文件自动下载）
- **macOS / Linux：** 核心搜索功能完整可用。但 OneDrive 文件自动下载依赖 Windows 系统特性，Mac 上不生效
- **Shell 命令：** 以 Windows 为主（`dir /b` / `Select-String`），Mac 可用 `ls` / `grep` 替代

---

*版本：v3 · MIT-0 · 双通道检索 + 渐进式描述进化 + 自动降级*

---

# knowledge-retrieval — Local Knowledge Base Search Skill

> A local document search skill designed for knowledge workers and consultants.
> For people who have accumulated years of PPTs, PDFs, and reports on their local drive and want AI-powered search — without moving anything to the cloud.

---

## Who this is for

You, if:

- You have **10+ years of work documents** sitting on your local hard drive
- Your files are **mostly PPTs, PDFs, scanned documents, and images** — not clean Markdown or plain text
- You want AI to make these materials searchable, but you **don't want to upload them to the cloud, convert formats, or set up a vector database**
- You usually search with **precise keywords or tags**, but sometimes want to ask naturally: "find me the report about X from last quarter"

If that sounds familiar, this skill is built for your workflow.

---

## What it solves

### 🔑 It reads your actual file formats

Most search tools expect plain text. This one handles what you actually have:

| Format | Support |
|--------|---------|
| PPTX | Full text extraction (nested shapes, speaker notes) |
| PDF | Dual-engine fallback (pdfminer → PyMuPDF) |
| DOCX / XLSX | Full text extraction |
| TXT / MD / CSV | Yes |
| Images | Embedded text recognition |

Files are read in place — your original folder is never modified. The knowledge base only creates **two-way shortcuts** pointing to your original files. For non-plain-text files (PDFs, PPTXs, DOCXs), extracted text is cached in a separate skill working directory, never mixed into your original folders.

### 🔑 Local-first

Your documents stay on your machine. The search index is built locally. Nothing is uploaded anywhere.

For many consultants this isn't a preference — it's a **compliance requirement**. Client materials cannot be sent to third-party services.

**Cloud-synced folders work too.** If your files live in OneDrive or similar, they are automatically downloaded when indexing or searching. Internet is required for the first full index build.

### 🔑 Dynamic updates

Your knowledge base changes every day — new files added, old ones revised. The index updates incrementally:

- New files → discovered on the next search automatically
- Modified files → descriptions auto-refresh
- No need to rebuild the entire index after every change

Most tools are "index once and freeze." This tool evolves with your files.

### 🔑 Keyword precision + natural language, both in one pass

Keyword search fails when the same concept uses different wording. Pure semantic search is fuzzy and requires maintaining a vector database.

Our approach: **AI expands your query into synonyms first, then hands it to a lightweight keyword index for precision matching.**

```
You ask: "find me hydrogen industry chain analysis reports"
     ↓
AI expands synonyms (up to 20 terms):
  "hydrogen" → hydrogen, H2, hydrogen energy
  "industry chain" → industry chain, supply chain, value chain
  "analysis" → analysis, assessment, study, whitepaper
     ↓
Keyword index searches with the expanded set
     ↓
AI semantic channel cross-checks results
     ↓
Combined ranking
```

You don't need to guess what exact words the author used. If the concept is right, AI and the keyword engine will find it together.

### 🔑 Gets smarter with use

Traditional approach: 30-minute initialization → fixed search quality forever.

Our approach: **Only the minimal index is built on first use.** Every time AI reads a file, it updates the file's description. Over time:

- Most-searched files → richest descriptions → highest hit rate
- Rarely accessed files → no wasted preprocessing
- Your actual search patterns → shape the index to serve you better

**It improves as you use it.**

### 🔑 Graceful degradation when things are missing

- No PDF library installed? Search still runs — PDF files just won't be found this time.
- BM25 index corrupted? Falls back to pure AI semantic matching automatically.
- No matching files found? AI honestly reports nothing — no hallucination.

You don't need to worry about the tool breaking. It handles its own edge cases.

---

## Comparison with similar tools

| Dimension | file-search | semfind | Ours |
|-----------|-----------|---------|------|
| PPT/PDF support | ❌ | ❌ | ✅ Full extraction |
| Local-first | ✅ | ✅ | ✅ |
| Dynamic updates | ✅ | ❌ One-time index | ✅ Incremental |
| Search method | Pure keyword | Pure semantic | Keyword + semantic dual-channel |
| Gets smarter | ❌ | ❌ | ✅ Description evolution |
| Graceful degradation | Crashes | Crashes | ✅ Fallback on every path |
| Initial setup time | Seconds | Minutes-hours | Seconds (progressive) |

---

## Quick start

```bash
# Install
openclaw skills install knowledge-retrieval

# The agent auto-detects your knowledge base folder
# No manual configuration needed
```

## Requirements

```bash
pip install bm25s pdfminer.six python-pptx
# Optional: python-docx (for DOCX support)
```

## Platform compatibility

- **Windows:** Full features, including OneDrive auto-download
- **macOS / Linux:** Core search works fully. OneDrive auto-download is Windows-specific
- **Shell commands:** Windows-native (`dir /b` / `Select-String`); Mac/Linux equivalents available (`ls` / `grep`)

---

*Version: v3 · MIT-0 · Dual-channel retrieval + progressive description evolution + graceful degradation*
