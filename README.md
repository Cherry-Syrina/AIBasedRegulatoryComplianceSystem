# 📑 AI-Powered Regulatory Compliance Checker for Contracts

An end-to-end AI system that reads legal contracts (PDF), breaks them down into clauses, flags compliance risks against major regulatory frameworks (GDPR, HIPAA, SOX, PCI-DSS, CCPA, and more), and generates actionable, AI-written suggestions — all through an interactive Streamlit dashboard.

**🔗 Live Demo:** [aibasedregulatorycompliancesystem.streamlit.app](https://aibasedregulatorycompliancesystemgit-zskpeytz4vvuolxgjynezs.streamlit.app/)

---

## ✨ Key Features

- **📄 PDF Ingestion & Semantic Chunking** — Extracts raw text with PyMuPDF and splits contracts into meaningful clauses using LangChain's `SemanticChunker` with HuggingFace `sentence-transformers` embeddings (`all-MiniLM-L6-v2`), with a paragraph-based fallback for edge cases.
- **🏷 Clause Classification** — Every clause is auto-tagged into categories such as Payment, Termination, Liability, Confidentiality, IP, Data Protection, Dispute Resolution, Force Majeure, and more, plus a regulatory-relevance score (`high` / `medium` / `low` / `minimal`).
- **⚖ AI-Powered Risk Analysis** — Each clause is sent through an LLM pipeline that identifies the relevant regulation (GDPR, HIPAA, SOX, PCI-DSS, CCPA, Export Controls, etc.), describes the specific compliance risk, and assigns a severity level (Low / Medium / High).
- **💡 Actionable Suggestions** — A second LLM pass generates concise, practical recommendations for reducing the risk in each flagged clause.
- **📝 Auto-Generated Safe Contract Report** — High-risk clauses are automatically rewritten into safer, compliant language and exported as a downloadable Word (`.docx`) report.
- **📊 Interactive Dashboard (5 modules)**:
  - **Home** — overview and quick-start guide
  - **Upload & Analyze** — upload a contract, track live progress, and view summary risk metrics
  - **Charts & Insights** — risk distribution (bar + pie), risk trends over time, and clause-length distribution (Matplotlib/Seaborn)
  - **Detailed Results** — searchable, filterable clause-level table with CSV/Excel export
  - **Company Dashboard** — historical analysis tracking via SQLite, with portfolio-level metrics and export/delete options
- **📧 Email Summary Reports** — Sends a formatted HTML summary of the risk analysis to a recipient's inbox via Gmail SMTP (yagmail).
- **📤 Google Sheets Export** — Optionally pushes clause-level results to a connected Google Sheet via a service account.
- **🔄 Multi-LLM Fallback Architecture** — Primary inference via Groq (LLaMA-3.3-70B), with automatic fallback to Gemini 2.5 Pro/Flash if a call fails, times out, or is rate-limited — including Gemini safety-filter auto-recovery.
- **🎨 Custom UI** — Dark, gradient-themed Streamlit interface with styled cards, tabs, and alerts.

---

## 🛠 Tech Stack

| Layer | Technology |
|---|---|
| Frontend / App | Streamlit |
| PDF Processing | PyMuPDF (fitz) |
| Chunking & Embeddings | LangChain, HuggingFace `sentence-transformers` |
| LLMs | Groq API (LLaMA-3.3-70B), Google Gemini API (2.5 Pro/Flash) |
| Data & Visualization | Pandas, Matplotlib, Seaborn, NumPy |
| Storage | SQLite (analysis history), Google Sheets API (optional) |
| Reporting | python-docx, xlsxwriter |
| Email | yagmail (Gmail SMTP) |

---

## 📂 Project Structure

```
├── app.py               # Main Streamlit app — UI, tabs, charts, email & history logic
├── ingestion.py          # PDF text extraction + semantic chunking + clause classification
├── analysis.py           # Batched LLM-based risk & regulation analysis
├── suggestions.py        # Batched LLM-based remediation suggestions
├── modifier.py           # Rewrites high-risk clauses and builds the Word report
├── save_to_sheets.py      # Combines results & exports to Google Sheets
├── llm_helper.py          # Groq/Gemini API wrappers with retries & fallback chain
├── config.py              # Centralized model, API, safety & batching configuration
├── requirements.txt
└── secrets_template.toml  # Template for Streamlit secrets
```

---

## ⚙ How It Works

1. **Upload** a contract PDF on the *Upload & Analyze* tab.
2. The text is extracted and split into **semantic clauses**, each classified by type and regulatory relevance.
3. Clauses are batched (to respect LLM token limits) and sent to **Groq → Gemini fallback chain** for risk analysis — identifying applicable regulations and severity.
4. A second LLM pass generates **remediation suggestions** for each clause.
5. High-risk clauses are **automatically rewritten** into safer language and compiled into a downloadable Word report.
6. Results are visualized across the **Charts & Insights** and **Detailed Results** tabs, saved to a local SQLite history, and optionally emailed or pushed to Google Sheets.

---

## 🚀 Setup & Run Locally

```bash
git clone https://github.com/Cherry-Syrina/AIBasedRegulatoryComplianceSystem.git
cd AIBasedRegulatoryComplianceSystem
pip install -r requirements.txt
```

Create a `.streamlit/secrets.toml` file (see `secrets_template.toml`) with your API keys:

```toml
GROQ_API_KEY = "your_groq_api_key_here"
GEMINI_API_KEY = "your_gemini_api_key_here"
SENDER_EMAIL = "your_gmail_address@gmail.com"
SENDER_PASSWORD = "your_gmail_app_password_here"

# Optional - only needed for Google Sheets export
# SHEET_ID = "your_google_sheet_id_here"
# GOOGLE_APPLICATION_CREDENTIALS = "path_to_service_account.json"
```

Then run:

```bash
streamlit run app.py
```

---

## 📋 Supported Regulations

GDPR · HIPAA · SOX · PCI-DSS · CCPA · FDA · EMA · Export Controls · Employment Law · Tax · General Legal

---

## 📌 Notes

- The first run downloads the `sentence-transformers` embedding model (~90MB); subsequent runs are cached.
- Google Sheets export requires a configured service account — the rest of the app works without it.
- PDF export of the rewritten contract isn't supported on cloud deployments (requires Microsoft Word); the Word `.docx` report works everywhere.

---

Made with ❤️ by Sushma Shukla
