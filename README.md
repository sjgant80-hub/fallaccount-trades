# FallAccount Trades

**A sovereign accounts app for UK builders, plumbers, sparkies, plasterers and other sole-trader trades.**

> A proper business. Ivan's tool. Yours after you fork it.

One HTML file. Opens in any browser. Lives on your phone. Your data never leaves your device.

No subscription. No login. No cloud. No telemetry. No nonsense.

**Live demo:** _drop your GitHub Pages URL here once enabled_

---

## What it does

| Workflow | Time | What you do |
|----------|------|-------------|
| 📸 Snap a receipt | 5 sec | Photo at Screwfix → auto-categorised → logged |
| 🚐 Log a journey | 10 sec | "Mr Smith's → Travis Perkins, 12 miles" → HMRC rate auto-calculated |
| 🔨 Start a job | 30 sec | "Kitchen refit — Mr Smith, £4,500 quoted" |
| 📤 Invoice it | 1 min | Job marked done → invoice generated → CIS deducted automatically |
| £ Tax position | live | Income, expenses, mileage, CIS — running total all year |
| 📑 MTD quarterly | 2 min | Pick the quarter → generate the figures → download |

### Built for actual site use

- **Big buttons** — usable with gloves, dirty hands, fat fingers
- **High contrast dark mode** — readable in sun, in a basement, at six in the morning
- **Works offline** — no signal in a basement? Still works.
- **Mobile-first** — designed for one-thumb use on a phone
- **Print-ready invoices** — generate a clean PDF, send via email/WhatsApp/text

### Built for actual trades

- **CIS handling both directions** — receiving (subcontractor) and paying (contractor). Most apps cock this up.
- **Simplified vehicle expenses** — 45p first 10k miles, 25p after, baked in
- **UK 2025/26 tax engine** — personal allowance, basic/higher/additional bands, PA taper, NI Class 4 (used as 2026/27 proxy)
- **VAT settings** — standard rate, flat rate scheme (9.5% labour-only or 14.5% general construction)
- **"Set aside £X per month"** — divides estimated tax bill by months remaining until Jan 31

---

## How to use it (5 minutes)

### On your phone

1. Open the link in your phone's browser (Chrome on Android, Safari on iPhone)
2. Tap the share/menu button → **"Add to Home Screen"** (looks and acts like an app after that)
3. Tap "Add receipt" → snap a photo or type the vendor + amount → done

That's it. No account, no email, no card.

### First time

- Open Settings (cog icon, top right)
- Set your **brand name** and **mark** (one letter — Ivan would put "I")
- Set your **CIS status** (none / 20% / 30% / gross)
- Set your **VAT status** (none / standard / flat rate)
- Optionally paste a **free Gemini API key** for smart photo-of-receipt scanning (get one [here](https://aistudio.google.com/apikey) — completely free, stays on your phone)

### Daily

- Add receipts as you go (Screwfix, Toolstation, Travis Perkins, fuel, etc.)
- Log mileage between jobs
- Create jobs to track materials + labour + travel against quoted price
- Invoice from finished jobs in one tap

### Monthly

- Tap **Home** dashboard — check the "Set aside for tax" number
- Tap **Tax** → check the running position

### Quarterly

- Tap **Tax → Generate quarterly report**
- Pick the quarter (Q1 = Apr-Jul, Q2 = Jul-Oct, Q3 = Oct-Jan, Q4 = Jan-Apr)
- Download the numbers
- Send to your accountant, or submit via HMRC's MTD portal

---

## Sovereignty — what this actually means

Every other accounting app stores your data on **their** server. You pay a monthly fee. If you stop paying, you lose access. If they get hacked, your receipts get leaked. If they get bought by a bigger company, the terms change overnight.

FallAccount Trades stores **everything on your phone** — IndexedDB and localStorage. We have no server. We cannot see your data. We cannot lose your data. We cannot sell your data. **There is no "we"** — the file is yours.

- ✅ Works offline indefinitely
- ✅ Survives our company dying / disappearing
- ✅ Survives the internet going down
- ✅ Survives any platform shutting us down
- ✅ Backup via JSON export — keep on USB or email to yourself
- ✅ Restore by importing the JSON — your phone restored exactly

The trade-off: if your phone dies and you didn't back up, the data goes with it. Same as a paper notebook in your van getting wet. **Back up monthly. There's a button for it.**

---

## For trades-mates: forking your own

Got mates who'd want one? Three options:

### Option 1 — free favour
Help your mate make their own copy. Five minutes of clicking. They get their own version with their brand name on it. You earn a beer.

### Option 2 — set it up for them (£50–£100 cash)
- Fork this repo to a new repo with their name (e.g. `dave-plastering/my-accounts`)
- Drop their logo / brand in via Settings
- Enable GitHub Pages on their repo
- Send them the link to bookmark
- Hand them a printed copy of the [intro doc](FOR-IVAN.md)
- An hour's work · cash in hand · they get a tool they'd otherwise pay £30/month forever

### Option 3 — refer them
Send them to whoever made yours. They'll get one set up. You earn a credit.

---

## For developers: how this is built

### Architecture

- **Single HTML file** — ~65 KB, no build step, no npm
- **Vanilla JS** — no React, no Vue, no framework
- **IndexedDB** for persistence with localStorage fallback
- **BroadcastChannel** for cross-tab + mesh interop (`fall-signal` prime 31)
- **Konomi licence shim** baked in (sovereign tier, no gate)
- **Optional Gemini Vision** for photo-of-receipt parsing (T3 cascade)

### Stack

- HTML + CSS + JS in one file
- [Syne](https://fonts.google.com/specimen/Syne) headings via Google Fonts
- System UI body font for fastest load
- PWA manifest baked into the HTML via data: URL (no separate files needed)

### Files

```
fallaccount-trades/
├── index.html        # the deliverable · 65 KB · single file
├── README.md         # this file
├── FOR-IVAN.md       # the human-friendly intro (rename for your audience)
├── LICENSE           # MIT
└── .gitignore
```

### Customising for a specific trade

Edit `index.html` directly. Things you'll probably want to change:

- **`RECEIPT_PATTERNS`** (line ~620) — vendor recognition rules. Add your local merchants.
- **`TAX` constants** (line ~510) — update for new tax years (annual review)
- **`state.settings.brand`** defaults — change the default brand name
- **Categories** in receipt modal — add domain-specific categories
- **Mileage rate** — currently HMRC 45p/25p; change if needed for other countries

### Forking workflow

```bash
# Fork this repo
gh repo fork sjgant80-hub/fallaccount-trades --clone=true

# Change the brand defaults in index.html
sed -i 's/FallAccount/MyTrades/g' index.html

# Enable Pages
gh repo edit --enable-pages --pages-branch main

# Push
git add . && git commit -m "branded for me" && git push
```

### Verify before deploy

```bash
node -e "
const html = require('fs').readFileSync('index.html', 'utf8');
console.log('size:', (html.length/1024).toFixed(1), 'KB');
const lastScript = html.lastIndexOf('<script>');
const code = html.substring(lastScript + 8, html.lastIndexOf('</script>'));
new Function(code);
console.log('parses: ok');
"
```

### The 7 layers (v19 spec)

For folks already in the Fall* ecosystem:

| Ring | Role | Implementation |
|------|------|----------------|
| L1 FACE | 5 views | Dashboard · Receipts · Jobs · Invoices · Tax |
| L2 SWARM | Ω + 5 agents | α scanner · β invoicer · γ sequencer · δ tax · ε MTD |
| L3 CASCADE | T0 → T3 | Regex parser offline → Gemini Vision online (optional) |
| L4 BLOOM | Routing | Vendor patterns → categories → tax buckets |
| L5 PERSIST | IndexedDB | + localStorage fallback + JSON export/import |
| L6 SKIN | Mobile-first | Dark default · amber `#e8a012` · Syne + system-ui · big buttons |
| L7 ASS | Empty state | "Drop your first receipt" — no setup wizard |

Cross-tool interop: `BroadcastChannel('fall-signal')` · prime 31 · hello broadcast on boot.

---

## Roadmap (informal)

- ✅ Core: receipts · jobs · invoices · mileage · tax · MTD
- ✅ CIS both directions
- ✅ Print-ready invoice PDF
- ✅ Optional Gemini vision receipt scan
- ⬜ Direct MTD submission via HMRC API (currently exports to text for accountant)
- ⬜ Bank statement import (CSV → auto-match against invoices/receipts)
- ⬜ Self-Assessment (SA100) export
- ⬜ Multi-currency (for trades working cross-border)
- ⬜ Photo storage for receipts (currently parses, doesn't store image)

PRs welcome. Especially from working tradesmen — if a workflow feels wrong on site, open an issue. The point is it works in the van, not in a developer's head.

---

## Licence

MIT — see [LICENSE](LICENSE). Use it, fork it, sell setup services to your mates, customise it for a different country or trade. No royalties, no restrictions.

The architecture (Fall*, Konomi, mesh primes) is part of a larger sovereign-tool estate at [sjgant80-hub](https://github.com/sjgant80-hub). This is one tool in that estate, configured for trades.

---

## Credit

- **Architecture:** Simon Gant · [@sjgant80-hub](https://github.com/sjgant80-hub) · [LinkedIn](https://www.linkedin.com/in/simon-gant-295b56180/)
- **Bloom decomposition library** (used in routing): [teslasolar/MissCassandra](https://github.com/teslasolar) · Thomas Frumkin
- **For Ivan** (and his mates) — the reason this exists.

◊·κ=1 · sovereign · single file · your data your phone · the mesh grows one builder at a time
