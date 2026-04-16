# 🧠 Classic RAG vs PageIndex: A Comparative Study

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-LLaMA%203.1-F55036?style=for-the-badge&logo=groq&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-Vector%20Search-0078D4?style=for-the-badge&logo=meta&logoColor=white)
![Notebook](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)

<br/>

> **"Semantic similarity is not enough for structured documents."**

<br/>

A hands-on comparison of two RAG strategies on a real-world structured document —  
revealing **when vector math fails** and **why reasoning wins**.

[📖 Read the Medium Article](#) · [🚀 Run on Colab](#) · [⭐ Star this repo](#)

</div>

---

## 🎯 What This Project Is About

Most RAG tutorials stop at vector search. This one goes further.

This notebook runs **two retrieval pipelines side-by-side** on the same document and the same question — then shows you exactly **why one wins and the other doesn't**.

| | Classic RAG | PageIndex RAG |
|:--|:--:|:--:|
| **Core Idea** | Match by meaning | Navigate by structure |
| **Mechanism** | Cosine similarity (FAISS) | LLM reasoning over a tree |
| **Mental Model** | Google Search | Human reading a Table of Contents |
| **Retrieval Unit** | Overlapping text chunks | Exact document node |
| **Noise** | ⚠️ High (irrelevant chunks) | ✅ Zero (pinpoints one section) |
| **LLM Backend** | `llama-3.1-8b-instant` via Groq | `llama-3.1-8b-instant` via Groq |

---

## 🏗️ Architecture Overview

```
📄 Document (Hospital SRS)
         │
   ┌─────┴──────┐
   ▼            ▼
🔵 Classic RAG  🟢 PageIndex RAG
   │            │
   ▼            ▼
Chunking     Build Tree Index
   │            │
   ▼            ▼
Embeddings   LLM Navigates Tree
(MiniLM-L6)     │
   │            ▼
   ▼         Exact Section Found
FAISS Index     │
   │            ▼
   ▼         Generate Answer
Top-K Chunks    │
   │            │
   └─────┬──────┘
         ▼
    📊 Compare Results
```

---

## 🔬 The Experiment

**Document used:** A simulated Hospital Management System — Software Requirements Specification (SRS).  
Chosen because it's a **deeply hierarchical, structured document** — the exact type that exposes Classic RAG's weaknesses.

**Test query:** *"How is patient data encrypted?"*

### 🔵 Classic RAG Result
- Retrieves **3 overlapping chunks** using cosine similarity  
- Some chunks contain irrelevant content (noise)  
- Answer quality depends on luck of the chunk boundary

### 🟢 PageIndex RAG Result
- Navigates the document tree using **LLM reasoning**  
- Arrives directly at `Section 3.1: Data Encryption`  
- Zero noise — one section, one precise answer

---

## 📦 Tech Stack

| Component | Tool | Why |
|:--|:--|:--|
| **LLM** | `llama-3.1-8b-instant` via [Groq](https://groq.com) | Ultra-fast inference, free tier |
| **Embeddings** | `all-MiniLM-L6-v2` via Sentence-Transformers | Lightweight, local, free |
| **Vector Search** | FAISS (IndexFlatL2) | Cosine similarity at scale |
| **Visualization** | Custom SVG renderer (pure Python) | No external viz dependencies |
| **Document** | Hospital SRS (simulated) | Real-world hierarchical structure |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8+
- A free [Groq API key](https://console.groq.com)

### Installation

```bash
git clone https://github.com/Rozera-xalil/PageIndex_Vs_HybridRAG_2-2-1
cd rag-vs-pageindex
pip install faiss-cpu sentence-transformers numpy requests
```

### Run the Notebook

```bash
jupyter notebook RAG_vs_PageIndex_Groq.ipynb
```

> **Note:** Replace `GROQ_API_KEY = "*"` in Step 3 with your actual Groq API key before running.

---

## 📓 Notebook Structure

| Step | Description |
|:--|:--|
| **Step 1** | Install dependencies |
| **Step 2** | Imports & configuration |
| **Step 3** | Production-grade Groq LLM client (retry logic, usage tracking) |
| **Step 4** | Sample document — Hospital SRS |
| **Step 5** | 🔵 Classic RAG pipeline (chunking → embeddings → FAISS → answer) |
| **Step 6** | 🟢 PageIndex RAG pipeline (tree index → LLM navigation → answer) |
| **Step 7** | Side-by-side evaluation on 4 test queries |
| **Step 8** | Interactive SVG visualization of results |
| **Step 9** | Use-case guide — when to use which approach |

---

## 📊 Key Results

| Query | Classic RAG | PageIndex RAG |
|:--|:--:|:--:|
| "How is patient data encrypted?" | ⚠️ Partial (noise included) | ✅ Exact (Sec 3.1) |
| "What are the user roles?" | ⚠️ Mixed chunks | ✅ Exact (Sec 2.2) |
| "What is the system uptime SLA?" | ❌ Wrong section | ✅ Exact (Sec 5.2) |
| "How does appointment scheduling work?" | ✅ Decent | ✅ Exact (Sec 4.2) |

---

## 💡 When To Use Each

### 🔵 Classic RAG — Use when:
- Building **general-purpose chatbots** or FAQ systems  
- Working with **unstructured text corpora**  
- Documents are **short** with no clear hierarchy  
- Wikipedia-style or web-scraped content  

### 🟢 PageIndex RAG — Use when:
- Working with **technical documentation** (SRS, API docs)  
- Documents have a clear **chapter/section hierarchy**  
- Dealing with **legal contracts** or **academic papers**  
- **Precision and explainability** are critical  

---

## 🔮 What's Next

- [ ] **Multi-node reasoning** — handle cross-section questions  
- [ ] **Auto tree extraction** — use Groq to build the tree from any document automatically  
- [ ] **Hybrid approach** — combine vector search + tree navigation  
- [ ] **Evaluation framework** — RAGAS metrics for precision/recall  

---

## 📝 Related Article

This notebook is the companion code to the Medium article:

> **"Classic RAG vs PageIndex: Why Your Vector Database Fails on Structured Documents"**  
> *by Rozêra Xelîl*

[👉 Read it on Medium](#) ← *(link coming soon)*

---

## 👤 Author

**Rozêra Xelîl**  
AI Engineer · RAG Systems · LLM Applications · 2+2=1

[![Medium](https://img.shields.io/badge/Medium-Follow-black?style=flat-square&logo=medium)](https://medium.com/@rozxalil801)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/Rozera-xalil)


---

## ⭐ If This Was Useful

Give it a star on GitHub and a clap on Medium — it helps more people find it! 🙏

---

<div align="center">
<sub>Built with 🧠 curiosity and ☕ coffee</sub>
</div>
