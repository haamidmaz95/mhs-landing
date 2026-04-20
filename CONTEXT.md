# CONTEXT.md — MHS Landing Page Deployment

> **For Claude Code:** This is a handoff document. Read it fully before taking any action. Execute the tasks in the order specified. Ask the user for confirmation only where flagged.

---

## 👤 Owner / User

- **Name:** Hamid Mazumder
- **Email:** hamid@revgrow.app
- **GitHub username:** `hamidmaz` (confirm with user before pushing)
- **Agency:** RevGrow (portfolio.revgrow.app)
- **Client this project is for:** Brayan Ruiz — Founder, Manufactured Housing Sales (MHS)

---

## 🎯 What This Project Is

A conversion-focused opt-in landing page for Brayan Ruiz's **Manufactured & Modular Housing Sales Guide** — a 45-page PDF booklet aimed at manufactured housing sales reps.

**Goal of the page:** capture leads (name, email, phone, role) who download the free PDF guide, and push them into a free Skool community.

**Business model (full picture, for context):**
1. Free PDF → lead magnet
2. Free Skool community → nurture layer
3. Paid 7-day challenge ($47–$97) → front-end monetization
4. Paid Skool community ($49–$97/mo) → recurring revenue
5. High-ticket coaching/training → back-end

This landing page is Layer 1 + 2 of that funnel.

---

## 📦 What's in This Folder

- `index.html` — the complete landing page, single-file, logo inlined as base64, no external image dependencies
- `CONTEXT.md` — this file

That's it. No build step. No npm. Pure HTML + CSS + vanilla JS.

---

## 🎨 Design Details (so you don't accidentally break the aesthetic)

- **Fonts:** Fraunces (serif display) + Inter (body) — loaded from Google Fonts
- **Brand colors:**
  - `--brand: #1E5F8E` (MHS blue)
  - `--ink: #0F1B2D` (near-black)
  - `--paper: #F7F2E9` (warm cream background)
  - `--accent: #C9A96E` (gold)
- **Aesthetic direction:** refined editorial / premium real-estate brochure — NOT generic SaaS. Avoid purple gradients, avoid Tailwind-default styling, avoid Inter-on-white at all costs.
- **Logo:** embedded as base64 inside the `<img>` tag in `<nav class="nav">`. Don't extract it to a separate file unless the user specifically asks.

---

## ✅ Tasks to Complete (in order)

### Task 1 — Initialize git repo

```bash
cd <project-folder>
git init -b main
git config user.email "hamid@revgrow.app"
git config user.name "Hamid Mazumder"
git add -A
git commit -m "Initial commit: MHS opt-in landing page"
```

### Task 2 — Create GitHub repo and push

**Preferred method (if `gh` CLI is available):**
```bash
gh repo create mhs-landing --public --source=. --push --description "Conversion-focused opt-in landing page for Manufactured Housing Sales"
```

**Fallback method (manual):**
1. Ask the user to create a repo at https://github.com/new named `mhs-landing` (public, don't initialize with README)
2. Then:
```bash
git remote add origin https://github.com/hamidmaz/mhs-landing.git
git push -u origin main
```

**If `gh` isn't installed**, offer to install it first:
```bash
brew install gh && gh auth login
```

### Task 3 — Deploy to Vercel

Hamid already uses Vercel extensively (has Vercel MCP connected). Use the Vercel MCP tools if available, otherwise fall back to CLI.

**Via Vercel CLI:**
```bash
npm i -g vercel
vercel --prod
```

**Via Vercel MCP:** use the `deploy_to_vercel` tool to deploy the current project, linked to the GitHub repo just created.

Once deployed, capture the production URL and share it with the user.

### Task 4 — Add standard repo files (commit in one go after initial push)

Create these files, then commit and push:

**`.gitignore`:**
```
.DS_Store
.vscode/
.idea/
*.log
.vercel
.netlify
.env
.env.local
node_modules/
```

**`README.md`:**
```markdown
# MHS Opt-in Landing Page

Conversion-focused landing page for the Manufactured & Modular Housing Sales Guide.

## Stack
Pure HTML + CSS + vanilla JS. Single file. No build step.

## Deploy
Deployed on Vercel: [URL once deployed]

## Before going live
1. Wire form submission to GHL webhook (see index.html `TODO` comment)
2. Replace `https://www.skool.com/` with actual Skool community URL (appears 2x)
3. Set up GHL automation for PDF delivery on form submit

## Local preview
```bash
python3 -m http.server 8080
```
```

**`vercel.json`:**
```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "SAMEORIGIN" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ]
}
```

Commit: `git add -A && git commit -m "Add repo scaffolding: README, .gitignore, vercel.json" && git push`

---

## ⚠️ Things NOT to Touch

- **Do not modify `index.html`** unless the user specifically asks. The copy, layout, and design are already dialed in.
- **Do not "upgrade" to React/Next.js/Vite.** This is intentionally a single static file for maximum portability (GHL, Vercel, Netlify, GitHub Pages, S3 — all work).
- **Do not replace the base64 logo** with an external file. The user already had issues with missing logo paths. Keep it inlined.
- **Do not change colors, fonts, or copy** without asking.

---

## 🔧 Known TODOs Inside `index.html` (for user to complete later, NOT you)

Search `index.html` for these — they're placeholders the user needs to wire up after deployment:

1. **GHL webhook URL** — around the `fetch('YOUR_GHL_WEBHOOK_URL'...)` block in the `<script>` section. Currently just `console.log`s. User will wire this to GHL.
2. **Skool community URL** — `https://www.skool.com/` appears twice (secondary CTA button and success state button). User will swap once the community is created.
3. **PDF delivery** — the success message says "check your inbox." User needs to build the GHL automation: form submit → contact created → email with PDF attached.

**Do not attempt to fix these automatically.** Flag them to the user in your final summary so they know what's left.

---

## 🧭 Final Summary Format

When done, report back to the user with:

1. ✅ GitHub repo URL
2. ✅ Vercel production URL
3. 📋 Three manual TODOs remaining (GHL webhook, Skool URL, PDF automation) — remind them
4. 💡 Next suggested step (offer to draft the GHL workflow OR the welcome email sequence)

Keep the summary tight — Hamid values concise output and moves fast.

---

## 📝 User Preferences (per Hamid's operating style)

- Direct, no fluff
- No excessive apologies or hedging
- Show the commands/outputs, don't over-explain
- If something fails, diagnose and retry — don't just report the failure
- He's on Mac (zsh), runs terminal from `~` by default, and tends to download to `~/Downloads`
