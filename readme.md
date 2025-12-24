# 🛡️ Dianping AI Review Radar  
*(AI-Powered Negative Review Summarizer)*

> **Efficiency, not decision-making**: Help users quickly identify potential “red flags” of a restaurant by compressing dozens of negative reviews into a clear, structured risk report.

---

## 📖 Background & Pain Points

When choosing a restaurant, users care more about **risk** than upside — because bad experiences define the lower bound of satisfaction.  
On Dianping, negative reviews are numerous, scattered, and highly emotional. To “avoid pitfalls,” users must read many individual reviews, which creates an **extremely high cognitive load**.

This project aims to use AI to automatically distill negative reviews into an **objective, structured “risk report”**, allowing users to understand the core issues in **10 seconds instead of reading 100 reviews**.

---

## 🛠️ Project Overview

This is a **Python + LLM–based automation demo**.

We selected the **first 25 negative reviews** of  
**Lihua Self-Service BBQ (Yuhua MixC branch)** on Dianping as a sample, and used **Google Gemini 3 Flash** for large-context reasoning and summarization.  
The final output is a **mobile-friendly HTML report** that highlights recurring problems and supporting evidence.

---

## 📂 Project Structure

```text
.
├── data/
│   └── reviews.json       # Raw negative reviews (Dianping sample)
├── main.ipynb             # Core logic (Jupyter Notebook)
├── report.html            # Generated AI risk report (HTML)
└── readme.md              # Project documentation
```
## 🚀 Technical Implementation

### 1) Model Invocation

- **Model**: `gemini-3-flash-preview` (via API)  
- **Goal**: Convert a large set of free-text negative reviews into structured JSON “risk points” suitable for frontend rendering.

---

### 2) Strategy: Map–Reduce (Divide & Conquer)

#### Map Stage — `chunk_process`

- Split the review list into chunks with `chunk_size = 13`
- Each chunk is independently sent to the model to:
  - Extract concrete complaints
  - Classify them into meaningful categories
- **Output**: structured fragments (JSON or near-JSON text)

> **Intuition**: Limiting each call to 13 reviews reduces context pressure and improves output consistency and stability.

---

#### Reduce Stage — `aggregate`

- Merge all chunk-level outputs:
  - Deduplicate overlapping issues
  - Merge semantically similar items
  - Normalize field names and structure
  - Aggregate statistics (e.g., mention count, mention rate)
- **Output**: a clean, standardized JSON schema, for example:  
  `categories → items → evidence_quotes`

---

### 3) Presentation Layer: Mobile-Friendly Interactive Cards

- Use **HTML + CSS templates** to render the JSON into a mobile-optimized UI
- **Core interaction design**:
  - **Collapsed by default**: shows tag + percentage + short description
  - **Tap to expand**: reveals supporting evidence (`evidence_quotes`) in a highlighted quote block

This design prioritizes **fast scanning first, evidence second**.

---

### 4) Final Outputs (Frontend-Oriented)

**Deliverables**:

- **Standardized JSON**
  - Designed for direct UI rendering
  - Clean hierarchy and consistent fields
- **Interactive HTML Report**
  - Mobile-first card layout
  - Click-to-expand evidence
  - Optimized for rapid “risk assessment”

---

## 🎯 Core Philosophy

- **Risk-first thinking**: Focus on what can go wrong, not marketing highlights
- **Compression over verbosity**: Turn many noisy reviews into a few clear signals
- **Assist, not replace**: AI reduces reading cost; the final judgment remains with the user
