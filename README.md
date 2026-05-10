# Knowledge Retrieval — 本地知识库检索 Skill

> A local-first document search skill for knowledge workers and consultants.
> Handles PPT/PDF/DOCX in place, searches with keyword + AI dual-channel.
> No cloud upload needed.
>
> 给知识工作者和顾问的本地文件检索方案。支持 PPT/PDF 等多格式、BM25+AI 双通道搜索、越用越聪明。

[![ClawHub](https://img.shields.io/badge/ClawHub-local--knowledge--retrieval-blue)](https://clawhub.ai/package/local-knowledge-retrieval)

---

## 📦 Install

```bash
openclaw skills install local-knowledge-retrieval
```

Or search "Knowledge Retrieval" in ClawHub.

## 🚀 Quick Start

```bash
# Build index from your files
openclaw run build-knowledge-index --kb-path ./my-docs

# Search
openclaw run search-knowledge --query "your question"
```

See full docs in [SKILL.md](SKILL.md) or visit the [ClawHub page](https://clawhub.ai/package/local-knowledge-retrieval).

## ✨ Features

| Feature | Description |
|---------|-------------|
| **Local-first** | All data stays on your machine. No cloud upload, ever. |
| **Dual-channel search** | BM25 keyword + AI semantic, cross-validated for accuracy |
| **PPT/PDF deep parse** | Extracts embedded graphics, speaker notes, tables from PPTX |
| **Progressive indexing** | Builds index incrementally, gets smarter with use |
| **Graceful degradation** | Missing dependencies = some features degrade, not crash |
| **Anti-hallucination** | Answers are grounded in retrieved documents, not LLM memory |

## 📋 Requirements

- Python 3.8+
- Optional: `pip install bm25s pdfminer.six python-pptx` (for full functionality)
- See [environment-setup.md](references/environment-setup.md) for details.

## ⚠️ Disclaimer

> This project is developed by a consultant, for consultants — built on real workflows. It solves real problems, not academic benchmarks. Maintained on a **best-effort basis**. PRs welcome, but don't expect instant responses to issues.

## 📄 License

MIT — do what you want, no strings attached.

---

*Built with 🐾 by Zara & 小爪*
