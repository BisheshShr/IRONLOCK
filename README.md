# Novyr — Accountability App

> Accountability · Discipline · Trust

Built with Firebase Realtime Database. No build step. Open `index.html` and go.

---

## Run in VS Code

1. Open this folder in VS Code
2. Install **Live Server** extension (Ritwick Dey)
3. Right-click `index.html` → **Open with Live Server**
4. Opens at `http://localhost:5500`

---

## Deploy to Vercel

### Fast way — Vercel MCP (recommended)
```bash
npx add-mcp https://mcp.vercel.com
```
This adds Vercel's MCP server to Claude — then just tell Claude:
> "Deploy this project to Vercel"
and Claude will handle it end-to-end.

### Manual CLI
```bash
npm i -g vercel
vercel --prod
# Follow prompts — no build config needed, it's a static HTML file
```

### Via GitHub
```bash
git init
git add .
git commit -m "Novyr v1"
gh repo create novyr-app --public --push
# Then: vercel.com/new → import repo → Deploy
```

---

## Payment Integrations

### eSewa + Khalti (Nepal) — MCP Servers
```bash
npx -y esewa-mcp    # https://www.npmjs.com/package/esewa-mcp
npx -y khalti-mcp  # https://www.npmjs.com/package/khalti-mcp
```
These open-source MCP servers let Claude initiate payments, verify transactions,
and look up details via eSewa/Khalti APIs directly. Sandbox credentials included.

In the app, the payment flow:
- Detects opponent's payment method from their profile
- Shows their eSewa/Khalti number with step-by-step instructions
- Deep-links into the native app (`esewa://`, `khalti://`)
- Notifies the opponent when payment is marked as sent

### PayPal
- Links to paypal.com/paypalme for quick transfers
- Opponent's PayPal email shown directly

---

## What's new in this version

- **Notifications tab** — real-time alerts when opponent relapses, joins, or pays
- **Red dot on nav** — unread notification indicator, polled every 12 seconds
- **Private leaderboards** — only shows battles you're currently in; once you leave, it's gone
- **In-app payment guide** — shows opponent's exact payment ID, step-by-step instructions, deep links to apps
- **Payment notification** — opponent gets notified when you mark a payment as sent
- **Renamed to Novyr**

---

## Firebase DB
```
https://accountability-21545-default-rtdb.asia-southeast1.firebasedatabase.app
```
Make sure Rules are set to `".read": true, ".write": true` for development.
