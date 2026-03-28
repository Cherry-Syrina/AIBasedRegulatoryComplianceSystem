# 🚀 Deployment Guide — AI Contract Compliance Checker

## What was fixed for deployment
- ✅ Removed hardcoded email credentials from app.py → now uses st.secrets
- ✅ Removed pythoncom import (Windows-only) from modifier.py
- ✅ Removed docx2pdf (requires Microsoft Word on Windows) from modifier.py
- ✅ Added yagmail and xlsxwriter to requirements.txt
- ✅ Removed docx2pdf from requirements.txt

---

## Step 1 — Get Your Free API Keys

| Service | Free Tier | Link |
|---|---|---|
| Groq | 14,400 req/day | https://console.groq.com |
| Gemini | 1,500 req/day | https://aistudio.google.com |
| Gmail App Password | Free | https://myaccount.google.com/apppasswords |

---

## Step 2 — Push to GitHub

1. Go to https://github.com and create a new **public** repository
   - Name it: `contract-compliance-checker`
2. Upload ALL files from this folder into the repo root:
   - app.py
   - analysis.py
   - config.py
   - ingestion.py
   - llm_helper.py
   - modifier.py
   - save_to_sheets.py
   - suggestions.py
   - requirements.txt

---

## Step 3 — Deploy on Streamlit Cloud (Free)

1. Go to https://share.streamlit.io
2. Sign in with your GitHub account
3. Click **"New app"**
4. Fill in:
   - Repository: `your-username/contract-compliance-checker`
   - Branch: `main`
   - Main file path: `app.py`
5. Click **"Advanced settings"** → open the **Secrets** tab
6. Paste the following (with your real values):

```toml
GROQ_API_KEY = "your_groq_api_key_here"
GEMINI_API_KEY = "your_gemini_api_key_here"
SENDER_EMAIL = "your_gmail@gmail.com"
SENDER_PASSWORD = "your_gmail_app_password"
```

7. Click **Deploy!**

Your app will be live at:
`https://your-username-contract-compliance-checker.streamlit.app`

---

## ⚠️ Notes

- **PDF download only (no PDF export):** The PDF export via docx2pdf requires Microsoft Word
  and only works on Windows. The Word (.docx) download works perfectly on all platforms.
- **Google Sheets:** This feature requires a Google Service Account JSON key.
  It will show an error if clicked without setup — the rest of the app works fine.
- **First load is slow:** The sentence-transformers model (~90MB) downloads on first run.
  Subsequent loads are cached and fast.
