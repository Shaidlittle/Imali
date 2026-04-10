# Imali — Deploy Guide

## What's in this folder

```
imali-deploy/
├── api/
│   └── chat.js        ← Vercel Edge Function (your secure API proxy)
├── public/
│   └── index.html     ← The full Imali app
├── vercel.json        ← Vercel routing config
├── package.json       ← Project metadata
└── README.md          ← This file
```

## How the proxy works

Users → imali.vercel.app → /api/chat (Edge Function) → Anthropic API

Your API key never touches the browser. It lives only in Vercel's encrypted environment variables.

---

## Deploy in 5 steps (free, ~10 minutes)

### Step 1 — Create a Vercel account
Go to https://vercel.com and sign up with GitHub (recommended) or email.

### Step 2 — Install Vercel CLI
Open your terminal and run:
```
npm install -g vercel
```

### Step 3 — Deploy
In your terminal, navigate to this folder and run:
```
cd imali-deploy
vercel
```

Follow the prompts:
- "Set up and deploy?" → Y
- "Which scope?" → your account
- "Link to existing project?" → N
- "Project name?" → imali (or anything you like)
- "In which directory is your code?" → ./ (just press Enter)
- "Override settings?" → N

Vercel will give you a URL like: https://imali-abc123.vercel.app

### Step 4 — Add your API key (CRITICAL)

Go to: https://vercel.com/[your-username]/imali/settings/environment-variables

Add this environment variable:
- Name:  ANTHROPIC_API_KEY
- Value: sk-ant-api03-YOUR-NEW-KEY-HERE
- Environment: Production + Preview + Development ✓

Click Save.

Get a fresh API key at: https://console.anthropic.com/settings/keys

### Step 5 — Redeploy to pick up the env var
```
vercel --prod
```

Your live URL: https://imali.vercel.app (after you set a custom domain)
or the auto-generated: https://imali-[hash].vercel.app

---

## Set a custom domain (optional but recommended)

1. Go to vercel.com → your project → Settings → Domains
2. Add your domain e.g. imali.co.za or getimali.co.za
3. Follow Vercel's DNS instructions (usually just add a CNAME record)

---

## Update the app later

Make changes to public/index.html, then run:
```
vercel --prod
```
That's it. Live in ~30 seconds.

---

## Cost

- Vercel hosting: FREE (Hobby plan, 100GB bandwidth/month)
- Anthropic API: ~$0.003 per conversation (Claude Sonnet)
  - 1,000 users × 10 messages/month ≈ $30/month
  - At R79/month per user: 1,000 users = R79,000 revenue vs ~R570 API cost

---

## Security notes

✓ API key is in Vercel env vars — never in the browser
✓ Each message is capped at 4,000 characters (prevents abuse)
✓ Conversation history capped at 20 turns (controls cost)
✓ max_tokens hard-capped at 1,000 (controls cost)
✓ CORS only allows your own domain

## Add spending limits (recommended)

In your Anthropic console, go to:
https://console.anthropic.com/settings/limits

Set a monthly spend limit of e.g. $50 so you never get a surprise bill while testing.
