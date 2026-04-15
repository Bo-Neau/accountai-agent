# 🔧 Setup Guide

Complete setup instructions for AccountAI Agent.

---

## Getting Your API Key

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign up or log in
3. Navigate to **API Keys** in the sidebar
4. Click **Create Key**
5. Copy the key — it starts with `sk-ant-`

> **Cost estimate:** Processing one document uses approximately 1,000–3,000 tokens. At current Claude claude-sonnet-4-20250514 pricing, this is roughly $0.003–$0.009 per document.

---

## Local Setup (5 minutes)

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/accountai-agent.git
cd accountai-agent

# 2. Open in browser
open src/accounting_agent.html
```

That's it. No `npm install`, no build step.

---

## Adding Your API Key to the Agent

Open `src/accounting_agent.html` in a text editor. Find this function:

```javascript
async function callClaude(systemPrompt, userContent, useVision = false) {
  const body = { ... };
  const resp = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body),
  });
```

Update the headers to include your key:

```javascript
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'sk-ant-YOUR-KEY-HERE',
  'anthropic-version': '2023-06-01',
  'anthropic-dangerous-direct-browser-access': 'true'
},
```

### Secure key prompt (recommended)

Instead of hard-coding, add this at the top of your `<script>` block:

```javascript
// Prompt for key once per session
if (!sessionStorage.getItem('anthropic_key')) {
  const key = prompt('Enter your Anthropic API key (starts with sk-ant-):');
  if (key) sessionStorage.setItem('anthropic_key', key);
}
const ANTHROPIC_KEY = sessionStorage.getItem('anthropic_key');
```

Then use `ANTHROPIC_KEY` in the headers instead of a hard-coded string.

---

## Testing the Setup

Use one of these test documents:
- Take a photo of any receipt from your phone
- Download a sample invoice PDF from the internet
- Screenshot an email invoice

Upload it and click **Run Agent**. You should see the 5-step pipeline complete in 10–30 seconds.

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `401 Unauthorized` | Check your API key is correct and has credits |
| `400 Bad Request` | File may be corrupted — try a different image |
| Pipeline stops at OCR | Image may be too blurry — use a clearer photo |
| Numbers don't extract | Make sure amounts are visible and not cut off |
| PDF not supported | Try converting PDF to PNG first |
| CORS error | Serve the file via `python -m http.server` instead of opening directly |

---

## Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 14+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Mobile Chrome | ✅ Works (camera capture supported) |
| Mobile Safari | ✅ Works |
