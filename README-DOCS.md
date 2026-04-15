# 🤖 AccountAI Agent

> **Autonomous AI accounting agent** — upload a photo of any bill or purchase order and Claude AI will extract all fields, generate double-entry journal entries (debit & credit), and build a live Profit & Loss report. Zero backend required.

![Agent Status](https://img.shields.io/badge/Agent-Online-brightgreen)
![Claude](https://img.shields.io/badge/Powered%20by-Claude%20claude-sonnet-4-20250514-orange)
![License](https://img.shields.io/badge/License-MIT-blue)
![No Backend](https://img.shields.io/badge/Backend-None%20Required-purple)

---

## 📸 What It Does

| Step | What happens |
|------|-------------|
| **1. Upload** | Drag & drop a photo of a bill, invoice, or purchase order (JPG, PNG, PDF) |
| **2. OCR Extract** | Claude Vision reads every field — vendor, date, line items, VAT, total |
| **3. Classify** | AI maps the document to the correct chart of accounts |
| **4. Validate** | Checks subtotal + tax = total, flags discrepancies |
| **5. Journal Entry** | Generates balanced double-entry DR/CR lines with account codes |
| **6. Approve & Post** | You approve — entries post to the live ledger |
| **7. P&L Report** | Profit & Loss statement auto-updates from posted entries |

---

## ✨ Features

### 🧠 AI Agent Characteristics
| # | Characteristic | Implementation |
|---|---------------|----------------|
| 1 | **Autonomy** | Decomposes "process this bill" into 5 sub-tasks automatically |
| 2 | **Reasoning & Planning** | Chain-of-thought trace visible in the agent log |
| 3 | **Tool Usage** | Calls Claude Vision API, chart of accounts DB, validation engine |
| 4 | **Memory** | Short-term (session) + long-term (vendor profiles, learned corrections) |
| 5 | **Learning & Adaptation** | Adapts account mappings from accountant corrections |
| 6 | **Multi-Modal** | Accepts JPG, PNG, PDF documents |
| 7 | **Self-Monitoring** | Detects amount discrepancies, retries on failure |
| 8 | **Fault Tolerance** | Graceful error handling with fallback classifications |
| 9 | **Security & Control** | Sandbox mode, permission toggles, approval workflows |
| 10 | **Human-AI Collaboration** | Accountant must approve before anything posts to ledger |

### 📊 Accounting Features
- **Double-entry bookkeeping** — every transaction balanced (DR = CR)
- **Chart of accounts** — 20 pre-mapped accounts across Assets, Liabilities, Equity, Revenue, Expenses
- **VAT handling** — auto-calculates Input VAT (7% Thailand default, configurable)
- **P&L report** — live profit & loss with revenue vs expense breakdown
- **CSV export** — download P&L report as spreadsheet
- **Journal ledger** — full audit trail of every posted entry

---

## 🚀 Quick Start

### Prerequisites
- An **Anthropic API key** — get one at [console.anthropic.com](https://console.anthropic.com)
- A modern web browser (Chrome, Firefox, Safari, Edge)
- No Node.js, no server, no database needed

### Installation

```bash
# Clone the repo
git clone https://github.com/YOUR_USERNAME/accountai-agent.git
cd accountai-agent
```

### Running the Agent

**Option A — Open directly in browser (simplest)**
```bash
open src/accounting_agent.html
# or double-click the file in your file explorer
```

**Option B — Serve locally (recommended for PDF support)**
```bash
# Python
python -m http.server 8080
# then open http://localhost:8080/src/accounting_agent.html

# Node.js
npx serve src/
```

**Option C — Deploy to GitHub Pages**

See [Deployment Guide →](#-deployment)

---

## 🔑 API Key Setup

The agent calls the Anthropic API directly from your browser. You have two options:

### Option 1 — Enter in browser (easiest, no code)
The browser will prompt for your API key the first time. It's stored in `sessionStorage` and never sent anywhere except Anthropic's API.

> To add this prompt, add the following snippet just before the closing `</script>` tag in `src/accounting_agent.html`:
> ```javascript
> // Add API key prompt at startup
> window.ANTHROPIC_KEY = sessionStorage.getItem('ak') || prompt('Enter your Anthropic API key:');
> if (window.ANTHROPIC_KEY) sessionStorage.setItem('ak', window.ANTHROPIC_KEY);
> ```
> Then update the `callClaude` function's headers to include:
> ```javascript
> 'x-api-key': window.ANTHROPIC_KEY,
> ```

### Option 2 — Hard-code for local use only
Open `src/accounting_agent.html` and find the `callClaude` function. Add your key to the headers:

```javascript
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'sk-ant-YOUR-KEY-HERE',   // ← add this line
  'anthropic-version': '2023-06-01',
  'anthropic-dangerous-direct-browser-access': 'true'
},
```

> ⚠️ **Never commit your API key to GitHub.** Use environment variables or the prompt method above for any shared/public deployment.

---

## 📖 How to Use

### Step 1 — Upload a Document
- Click the upload zone or drag & drop a file
- Supports: **JPG**, **PNG**, **PDF**
- Works with photos taken on your phone

### Step 2 — Run the Agent
Click **Run Agent ↗** and watch the 5-step pipeline execute:

```
👁 OCR Extract → 🏷 Classify → ✓ Validate → 📒 Journal → 📊 P&L Update
```

The **Agent Reasoning Trace** shows exactly what Claude is thinking at each step.

### Step 3 — Review Extracted Fields
The agent extracts:
- Document type, vendor name, document number
- Date, due date, currency
- Line items, subtotal, VAT, discount, total
- Payment terms and notes

### Step 4 — Approve or Reject
> The agent **always asks for your approval** before posting anything to the ledger.

- ✓ **Approve & Post** — posts journal entries to the ledger and updates P&L
- ✕ **Reject** — flags document for manual review, nothing is posted

### Step 5 — Review Journal Entries
Navigate to **Journal Entries** to see:
- All debit (DR) and credit (CR) lines
- Account codes and names
- Balance verification (DR total = CR total)

### Step 6 — View P&L Report
Navigate to **P&L Report** to see:
- Revenue vs expense breakdown
- Net profit / loss
- Profit margin %
- Export to CSV

---

## ⚙️ Agent Settings

| Setting | Default | Description |
|---------|---------|-------------|
| Human approval workflow | **ON** | Require sign-off before posting any entry |
| Auto-post high-confidence entries | OFF | Post without approval if confidence ≥ 95% |
| Learning & adaptation | **ON** | Agent improves from your corrections |
| Sandbox mode | OFF | Analyze documents without posting anything |
| Verbose reasoning trace | **ON** | Show full chain-of-thought in log |

---

## 🗂️ Chart of Accounts

The agent uses this default chart of accounts (Thai accounting standard):

| Code | Account | Type |
|------|---------|------|
| 1000 | Cash | Asset |
| 1100 | Accounts Receivable | Asset |
| 1170 | Input VAT | Asset |
| 1200 | Inventory | Asset |
| 2000 | Accounts Payable | Liability |
| 2100 | VAT Payable | Liability |
| 3000 | Retained Earnings | Equity |
| 4000 | Sales Revenue | Revenue |
| 4100 | Service Revenue | Revenue |
| 5000 | Cost of Goods Sold | Expense |
| 5100 | Purchases | Expense |
| 5200 | Office Supplies | Expense |
| 5300 | Utilities | Expense |
| 5400 | Professional Services | Expense |
| 5500 | Rent | Expense |
| 5600 | Software & Licenses | Expense |
| 5700 | Marketing | Expense |
| 5800 | Travel & Entertainment | Expense |
| 5900 | Miscellaneous | Expense |

To customize, edit the `COA` object in `src/accounting_agent.html`.

---

## 🌐 Deployment

### GitHub Pages (free hosting)

1. Push this repo to GitHub
2. Go to **Settings → Pages**
3. Set source to `main` branch, `/src` folder
4. Your agent will be live at `https://YOUR_USERNAME.github.io/accountai-agent/accounting_agent.html`

### Netlify (drag & drop)
1. Go to [netlify.com](https://netlify.com)
2. Drag the `src/` folder onto the Netlify dashboard
3. Done — live in 30 seconds

### Vercel
```bash
npx vercel src/
```

---

## 🏗️ Architecture

```
User uploads document (image/PDF)
         │
         ▼
┌─────────────────────────────────────────────────────┐
│                  Agent Pipeline                      │
│                                                     │
│  Step 1: Claude Vision API                          │
│  → OCR + structured JSON extraction                 │
│                                                     │
│  Step 2: Claude claude-sonnet-4-20250514 API                   │
│  → Document classification + account mapping        │
│                                                     │
│  Step 3: Local validation engine                    │
│  → Math checks, discrepancy detection               │
│                                                     │
│  Step 4: Claude claude-sonnet-4-20250514 API                   │
│  → Double-entry journal generation                  │
│                                                     │
│  Step 5: Local P&L engine                          │
│  → Account aggregation + report generation          │
└─────────────────────────────────────────────────────┘
         │
         ▼
Human approval → Post to in-memory ledger → Update P&L
```

**Tech stack:**
- Pure HTML + CSS + Vanilla JS — zero dependencies
- Anthropic Claude claude-sonnet-4-20250514 API (vision + text)
- All data stored in browser memory (session-based)

---

## 📁 Repository Structure

```
accountai-agent/
├── README.md                    ← You are here
├── LICENSE
├── .gitignore
├── src/
│   └── accounting_agent.html   ← The complete agent (single file)
└── docs/
    ├── SETUP.md                 ← Detailed setup guide
    ├── ACCOUNTING.md            ← Accounting concepts explained
    └── CUSTOMIZATION.md         ← How to customize the agent
```

---

## 🔒 Privacy & Security

- **No data leaves your browser** except to Anthropic's API for document processing
- **No database** — all data is in browser memory and clears on page refresh
- **No backend server** — purely client-side
- Anthropic's API processes images under their [privacy policy](https://www.anthropic.com/privacy)
- Never commit API keys to version control

---

## 🛠️ Customization

### Change VAT rate
Find in `src/accounting_agent.html`:
```javascript
// Change 7 to your country's VAT rate
'VAT rate: 7% (Thailand)'
```

### Add accounts to chart of accounts
```javascript
const COA = {
  // ... existing accounts ...
  '6000': { name: 'Equipment', type: 'Asset' },
  '6100': { name: 'Depreciation', type: 'Expense' },
};
```

### Change currency
Search for `฿` (Thai Baht) and replace with your currency symbol.

### Change language of extraction
In the `journalPrompt` and `classifyPrompt` strings, add:
```
Respond with account names in Thai/English/your language.
```

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit changes: `git commit -m 'Add my feature'`
4. Push: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — see [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgements

- [Anthropic](https://anthropic.com) — Claude AI API
- Built for accountants who want AI superpowers without complex software

---

<div align="center">
  <strong>Built with Claude AI · Zero backend · Open source</strong>
</div>
