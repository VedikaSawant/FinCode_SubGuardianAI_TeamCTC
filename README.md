# 🛡️ SubGuardianAI

> **LLM-powered subscription leakage detector for Indian users** — upload your bank or UPI statement and let AI find the subscriptions draining your wallet, score their value, and tell you exactly what to cancel.

Built by **Team CTC** for the **FinCode Hackathon**.

---

## 🧠 What It Does

Most people have no idea how many subscriptions they're silently paying for. DrainGuard AI parses your bank/UPI statement PDF, runs it through a multi-step LLM pipeline, and gives you a full picture — with scores, savings projections, and cancellation guides — in under a minute.

| Step | What Happens |
|---|---|
| 📄 Ingest | Parses PhonePe, HDFC, SBI, Axis, GPay PDFs or manual entry |
| 🔎 Detect | LLM identifies recurring subscriptions vs one-time purchases |
| 📊 Analyse | Flags forgotten subs, price hikes, duplicates, trial traps |
| ⚖️ Score | Worth-It Score (0–100) with reasoning for every subscription |
| 💸 Project | Calculates leakage, annual savings, and SIP growth projections |
| 💡 Insights | Personalised nudges, peer comparison, cancellation step guides |
| 📄 Report | Downloadable PDF audit report |

---

## 🖥️ Demo

![Dashboard showing health score, monthly drain, savings and subscription vault](screenshot.png)

---

## 🚀 Getting Started

### 1. Clone the repo
```bash
git clone https://github.com/your-username/drainguard-ai.git
cd drainguard-ai
```

### 2. Install dependencies
```bash
pip install pdfplumber pandas numpy rapidfuzz numpy-financial
pip install gradio plotly reportlab openai
```

### 3. Set your API keys

DrainGuard uses [OpenRouter](https://openrouter.ai) to access LLMs:
```python
import os
os.environ["OPENROUTER_API_KEY"] = "your_openrouter_key_here"
```
Get a free key at [openrouter.ai](https://openrouter.ai). The default model is `meta-llama/llama-3.3-70b-instruct:free` — no cost.

### 4. Run
```bash
python drainguard_fixed.py
```
Opens a Gradio UI at `localhost:7860`. Use `share=True` in `app.launch()` for a public link.

#### Running in Google Colab
```python
from google.colab.output import eval_js
print(eval_js("google.colab.kernel.proxyPort(7860)"))
app.launch(debug=True, prevent_thread_lock=True)
```
Use the Colab proxy URL — it has no timeout unlike the `share=True` link.

---

## 📂 Project Structure

```
drainguard-ai/
├── drainguard_fixed.py   # Full pipeline + Gradio UI (single file)
└── README.md
```

Everything lives in one file by design — easy to run in Colab without any setup overhead.

---

## ⚙️ Architecture

```
PDF / Manual Input
        ↓
  PDFParser           — regex-based PhonePe + generic bank parser
  DataNormalizer      — canonical merchant name resolution (RapidFuzz)
  NoiseFilter         — removes salary, EMI, ATM, refund transactions
        ↓
  AICore (LLM)
    ├── detect_subscriptions()   — identifies recurring charges
    ├── analyse_behaviour()      — forgotten / price hike / duplicate flags
    └── score_subscriptions()    — Worth-It Score 0–100
        ↓
  AnalyticsEngine
    ├── compute_leakage()        — monthly drain, wasted spend, health score
    ├── project_savings()        — annual savings + SIP projection (numpy-financial)
    └── llm_insights()           — narrative, peer comparison, nudges
        ↓
  AlertEngine         — upcoming renewals, forgotten subs, price hike alerts
  ReportGenerator     — PDF report (ReportLab)
        ↓
  Gradio Dashboard
```

### LLM Techniques Used
- **Structured JSON output** — strict schema enforced in every prompt
- **Chain-of-Thought (CoT)** — explicit reasoning steps before scoring
- **Few-shot examples** — 1–2 labelled examples per prompt
- **Context stuffing** — full transaction history passed to LLM
- **Chunking** — merchants split into batches of 10 to stay within token limits
- **Persona prompting** — "senior fintech analyst specialising in Indian spending"
- **Rule-based fallback** — if LLM scoring fails, Python heuristics take over

---

## 🔧 Configuration

### Changing the LLM model
In `LLMClient`:
```python
MODEL = "meta-llama/llama-3.3-70b-instruct:free"   # default (free)
MODEL = "google/gemini-flash-1.5"                   # faster
MODEL = "openai/gpt-4o-mini"                        # most reliable
```

### Manual entry (no PDF needed)
Enter subscriptions directly in the UI:
```
Netflix, 649
Spotify, 119
Cult.fit, 2499
Adobe, 4230
```

---

## 🇮🇳 Indian-first Design

- Amounts displayed in INR (₹)
- Knows Indian OTT platforms: Hotstar, ZEE5, SonyLIV, JioCinema
- Knows Indian SaaS, fitness, telecom: Cult.fit, HealthifyMe, Airtel, Jio
- Peer comparison benchmarked against average Indian urban millennial (₹1,800/month)
- SIP projection at 12% p.a. (standard Indian mutual fund benchmark)
- Cancellation guides tailored to Indian app flows

---

## 📊 Dashboard Features

- **Wallet Health Score** — 0 to 100
- **Monthly Drain & Annual Total** — total subscription spend
- **Wasted Monthly** — spend on low-score / forgotten subs
- **Savings/yr** — if recommended cancellations are made
- **SIP Projection** — 5-year and 10-year investment growth from savings
- **Subscription Vault** — every sub with score, verdict, flags, next renewal
- **Spend by Category chart** — OTT, SaaS, Fitness, Food-Delivery, etc.
- **Worth-It Score vs Cost scatter** — visual overview of all subs
- **Alert Centre** — upcoming renewals, forgotten subs, price hikes
- **Action Centre** — cohort label, peer comparison, cancellation guides

---

## 🙏 Acknowledgements

- [OpenRouter](https://openrouter.ai) — unified LLM API
- [Meta Llama 3.3](https://llama.meta.com) — open-source LLM
- [Gradio](https://gradio.app) — UI framework
- [pdfplumber](https://github.com/jsvine/pdfplumber) — PDF parsing
- [RapidFuzz](https://github.com/rapidfuzz/RapidFuzz) — merchant name matching

---

## 📄 License

MIT License — free to use, modify and distribute.
