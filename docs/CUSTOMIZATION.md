# 🛠️ Customization Guide

How to adapt AccountAI Agent for your business.

---

## Change Currency

Search for `฿` in `src/accounting_agent.html` and replace with your symbol:
- `$` — US Dollar
- `€` — Euro
- `£` — British Pound
- `¥` — Japanese Yen
- `₹` — Indian Rupee

Also update the currency in the extraction prompt:
```javascript
// Find this line in the visionContent prompt:
'currency': "THB|USD|EUR|etc"
// and update the VAT prompt:
'VAT rate: 7% (Thailand)'
```

---

## Change VAT Rate

Find all instances of `7%` and update:
```javascript
// In the journal generation prompt:
'VAT rate: 7% (Thailand)'
// Change to e.g.:
'VAT rate: 20% (UK - standard rate)'
```

---

## Add Custom Accounts

Edit the `COA` object:
```javascript
const COA = {
  // existing accounts...

  // Add your custom accounts:
  '1300': { name: 'Prepaid Expenses', type: 'Asset' },
  '2200': { name: 'Bank Loan', type: 'Liability' },
  '5010': { name: 'Direct Labor', type: 'Expense' },
  '6000': { name: 'Depreciation', type: 'Expense' },
};
```

---

## Multi-Language Support

To extract and display in a different language, update the system prompts:

```javascript
// In callClaude system prompt for OCR:
'You are an expert accountant AI. Respond in Thai. Extract...'

// In the journal prompt:
'Generate journal entries. Use Thai account names.'
```

---

## Persistent Storage

Currently all data clears on page refresh. To persist across sessions, add `localStorage`:

```javascript
// Save entries
function saveEntries() {
  localStorage.setItem('journal', JSON.stringify(state.journalEntries));
  localStorage.setItem('pnl', JSON.stringify(state.pnlAccounts));
}

// Load on startup
function loadEntries() {
  const j = localStorage.getItem('journal');
  const p = localStorage.getItem('pnl');
  if (j) state.journalEntries = JSON.parse(j);
  if (p) state.pnlAccounts = JSON.parse(p);
}

// Call loadEntries() at the end of your script
loadEntries();
```

Then call `saveEntries()` inside `approveEntry()`.

---

## Connect to a Real Database

To persist data in a real backend:

```javascript
// Replace the in-memory post logic in approveEntry() with:
async function postToBackend(entries) {
  const resp = await fetch('https://your-api.com/journal', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'Authorization': 'Bearer YOUR_TOKEN' },
    body: JSON.stringify({ entries, timestamp: new Date().toISOString() })
  });
  return resp.json();
}
```

Compatible with: Supabase, Firebase, Airtable, any REST API.

---

## Add More Document Types

The agent handles any document Claude can read. To add specific handling for new types, extend the classification prompt:

```javascript
const classifyPrompt = `...
Additional document types to handle:
- "Payroll Summary" → DR 5010 Direct Labor, CR 1000 Cash
- "Depreciation Schedule" → DR 6000 Depreciation, CR 1500 Accumulated Depreciation
- "Loan Statement" → DR 2200 Bank Loan, CR 1000 Cash
...`;
```
