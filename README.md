# IRONLOCK — NoFap Accountability App

> Accountability · Discipline · Trust

A full-featured accountability app with Firebase Realtime Database backend, friend competitions, monetary penalties, and Nepal + global payments.

---

## Quick Start in VS Code

### 1. Open in VS Code
```bash
code ironlock-vscode
# OR: File → Open Folder → select ironlock-vscode
```

### 2. Install Live Server extension
- Press `Ctrl+Shift+X` (Extensions)
- Search "Live Server" by Ritwick Dey
- Install it

### 3. Run the app
- Right-click `index.html` → **"Open with Live Server"**
- OR click "Go Live" in the bottom-right status bar
- Opens at `http://localhost:5500`

> **Note:** The app uses Firebase REST API directly — no npm install needed!

---

## Features

| Feature | Description |
|---------|-------------|
| **Accounts** | Register/login with email + password stored in Firebase |
| **Streak tracking** | Start fresh or enter existing clean days |
| **Solo Pledge** | Self-accountability: relapse = money penalty |
| **Friend Battle** | 1v1 via 6-digit invite code, real-time tracking |
| **Relapse logging** | Self-reported with honor confirmation |
| **Opponent notifications** | Opponent notified on every relapse |
| **Leaderboard** | All active battle participants ranked |
| **History** | Full relapse log with streak-at-time |
| **Debt tracking** | Cumulative penalty tracking + payment marking |

---

## Payment Methods

### eSewa (Nepal) 🇳🇵
- User links their eSewa phone number on signup
- On penalty: guided to open eSewa app and send the amount
- For automated payments: `npx -y esewa-mcp` (open source MCP server)
- Docs: https://www.npmjs.com/package/esewa-mcp

### Khalti (Nepal) 🇳🇵
- User links their Khalti phone number on signup
- On penalty: guided to open Khalti app and transfer
- For automated payments: `npx -y khalti-mcp` (open source MCP server)
- Docs: https://www.npmjs.com/package/khalti-mcp

### PayPal 🌍
- User links PayPal email on signup
- On penalty: direct link to PayPal send money page
- For automated payments: integrate PayPal JS SDK

> Both esewa-mcp and khalti-mcp include sandbox credentials for testing without real money.

---

## Firebase Database Structure

```
accountability-21545-default-rtdb/
├── users/
│   └── {email_key}/           # e.g. user_at_example_dot_com
│       ├── name
│       ├── email
│       ├── passHash           # base64 (upgrade to proper hashing in production)
│       ├── payMethod          # "esewa" | "khalti" | "paypal"
│       ├── payId              # phone number or email
│       ├── streakStart        # timestamp (ms)
│       ├── totalRelapses
│       ├── relapseLog[]       # [{time, streakBroken}]
│       ├── mode               # "solo" | "battle" | null
│       ├── challengeId        # 6-char code
│       ├── penalty            # amount per relapse
│       ├── currency           # "NPR" | "USD"
│       ├── totalDebt
│       ├── paidDebt
│       ├── notifications[]    # pending alerts from opponents
│       └── createdAt
└── challenges/
    └── {CODE}/                # e.g. "AB3X7K"
        ├── id
        ├── penalty
        ├── currency
        ├── createdAt
        └── players/
            └── {email_key}/
                ├── email
                ├── name
                ├── streakStart
                └── relapses
```

---

## Deploy to Vercel

### Option A: GitHub + Vercel (recommended)
```bash
git init
git add .
git commit -m "IRONLOCK v1.0"
gh repo create ironlock-app --public --push
# Then: vercel.com/new → import repo → deploy
```

### Option B: Vercel CLI
```bash
npm i -g vercel
vercel --prod
```

### Option C: Drag & Drop
- Go to vercel.com → New Project → drag this folder → Deploy

---

## Firebase Setup (already done for this project)

The app is wired to:
```
https://accountability-21545-default-rtdb.asia-southeast1.firebasedatabase.app
```

To use your own database:
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Create project → Realtime Database → Start in test mode
3. Copy your database URL
4. Replace `DB_URL` in `index.html` (line ~3 of the `<script>` block)

> **Security note:** For production, add Firebase Authentication and proper security rules. Current rules are open for development (`".read": true, ".write": true`).

---

## Production Checklist
- [ ] Enable Firebase Authentication (replace passHash system)
- [ ] Tighten Firebase security rules (users can only read/write their own data)
- [ ] Integrate eSewa/Khalti MCP for automated payment initiation
- [ ] Add push notifications (Firebase Cloud Messaging)
- [ ] Add email notifications on relapse events
