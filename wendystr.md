# WENDYSTR Website Build Prompt

---

## Project Brief

Build a single-page website for **WENDYSTR** — a Solana token that uses trading fees to programmatically buy Wendy's ($WEN) stock. The site must feel like a premium financial product, not a memecoin. Think Bloomberg Terminal meets a luxury crypto fund. Serious, data-forward, and slightly absurdist — it knows exactly how funny the concept is, but plays it completely straight.

---

## Tech Stack

- **Framework:** React (single file, no build step required) or Next.js
- **Styling:** Tailwind CSS
- **Stock Price API:** Finnhub.io (free tier, real-time US stocks) — ticker: `WEN`
  - Endpoint: `https://finnhub.io/api/v1/quote?symbol=WEN&token=YOUR_API_KEY`
  - Get a free key at finnhub.io — no credit card required
  - Poll every 15 seconds for live updates
- **Holdings Data:** Hardcoded variable at the top of the file (you will update this manually as stock is purchased). Format: `{ shares: 0, avgCost: 0.00, lastUpdated: "2025-01-01" }`

---

## Design Direction

**Palette — 5 colours only, no more:**
- Background: `#0A0A0A` (near black)
- Surface: `#111111` (card backgrounds)
- Border: `#1F1F1F` (subtle dividers)
- Red: `#DA291C` (Wendy's red — used sparingly, only on key numbers and CTAs)
- White: `#F0EDE8` (warm off-white for all body text)
- Muted: `#555555` (labels, secondary text)

**Typography:**
- Display/hero: `Playfair Display` (Google Fonts) — used only for the hero headline and the two live stat numbers. Heavy weight, large. This is the one expressive element.
- Body/UI: `Inter` (Google Fonts) — everything else. Tight tracking on labels (`letter-spacing: 0.08em`), sentence case everywhere.
- Data/numbers: `JetBrains Mono` (Google Fonts) — price tickers, percentage changes, share counts only.

**Design Rules:**
- Zero rounded corners anywhere — all cards and borders are sharp 90°
- No gradients
- No hero image or background texture
- Thin `1px` borders in `#1F1F1F` everywhere — not shadows
- All section padding generous: minimum `80px` top and bottom
- Mobile responsive — stack to single column below 768px

**The Signature Element:**
A live "heartbeat" line — a minimal horizontal SVG line that pulses with a single dot whenever the stock price refreshes. Sits directly below the live price display. Subtle, not flashy. This is the one animated element on the page — everything else is static.

---

## Page Structure

### 1. Navigation (sticky, minimal)
- Left: `WENDYSTR` in Inter, bold, `13px`, all-caps, tracked wide
- Right: `$WEN` pill showing current stock price in JetBrains Mono, updates live. Red dot indicator (●) pulsing when data is fresh
- No other nav links
- Background: `#0A0A0A`, `1px` bottom border `#1F1F1F`

---

### 2. Hero Section
**Full viewport height.**

Top label (small, muted, tracked): `AUTOMATED STOCK ACQUISITION PROTOCOL`

Main headline (Playfair Display, ~72px desktop, ~40px mobile):
> "Every trade buys a piece of the square."

Subheadline (Inter, ~18px, muted):
> WENDYSTR routes 90% of trading fees into programmatic purchases of Wendy's stock ($WEN). No team discretion. No manual execution. Just an automated pipeline converting crypto volume into equity ownership.

Two CTA buttons side by side:
- Primary (red background, white text): `Buy WENDYSTR`  → links to Pump.fun
- Secondary (transparent, white border, white text): `View on Explorer` → links to Solscan

Below buttons, small muted text:
> Launched on Pump.fun · Solana · 100,000,000 supply

---

### 3. Live Stats Bar
Full-width dark bar (`#111111`), `1px` top and bottom border. Three stats displayed inline, separated by vertical dividers:

| Label | Value | Notes |
|---|---|---|
| `WEN PRICE` | `$X.XX` | Live from Finnhub. JetBrains Mono. Show `▲` in red or `▼` in muted on change. |
| `SHARES OWNED` | `X,XXX` | From hardcoded holdings variable. JetBrains Mono. |
| `PORTFOLIO VALUE` | `$XX,XXX.XX` | Calculated: shares × current price. Updates live. |

Below the price, the heartbeat SVG pulse line.

---

### 4. How It Works
Three-column grid (stack on mobile). No numbered markers — use a thin red `2px` top border on each card instead as the visual anchor.

**Card 1 — FEES COLLECTED**
> Every buy and sell of WENDYSTR generates trading fees. These are routed automatically via on-chain workflow triggers — no human handles the funds.

**Card 2 — STOCK PURCHASED**
> 90% of fees are converted to USD and used to market-buy $WEN on the open market via automated brokerage API. Programmatic, documented, and traceable.

**Card 3 — SUPPLY BURNS**
> Sale proceeds from the reserve buy back WENDYSTR from the open market and send it to a dead wallet — permanently reducing supply and rewarding holders.

---

### 5. The Numbers (full-width, dark background)
Display four large stats in a 2×2 grid. Playfair Display for the number, Inter for the label below.

- `100,000,000` — Total Supply
- `90%` — Fees to Stock Reserve
- `1.25x` — Target Resale Multiplier
- `1 : 1` — Token to Vote Ratio

---

### 6. Live Holdings Table
Section title: `RESERVE HOLDINGS` (small, tracked, muted label above)
Headline: `What the treasury owns, right now.`

A clean table with columns: `ASSET` · `SHARES` · `AVG COST` · `CURRENT PRICE` · `MARKET VALUE` · `P&L`

Initially shows one row: Wendy's ($WEN) populated from the hardcoded variable + live price.
Table rows have `1px` bottom borders, no zebra striping, hover state is a very subtle `#161616` background.

Below table, small muted text:
> Holdings updated manually after each acquisition. All purchases verified on-chain.

---

### 7. Token Info Strip
Two-column layout: left side text, right side a clean monospace data block.

**Left:**
> WENDYSTR is not a fund. It is a token with a transparent, automated acquisition strategy. No promises, no projections — just a public wallet, a live table, and a bot that buys burgers.

**Right (monospace data block, styled like a terminal):**
```
NETWORK     Solana
TICKER      $WENDYSTR
SUPPLY      100,000,000
LAUNCH      Pump.fun
FEE SPLIT   90 / 9 / 1
BURN        Buyback & burn
GOVERNANCE  1 token = 1 vote
```

---

### 8. Footer
Minimal. Single row.
- Left: `WENDYSTR · 2025`
- Center: `Not financial advice. Not affiliated with Wendy's International.`
- Right: Links — `Pump.fun` · `Solscan` · `Twitter/X`

All footer text `11px`, muted colour.

---

## Interactions & Animation

- **Only one animation:** the heartbeat pulse line under the stock price. An SVG `<line>` with a small `<circle>` that slides along it and fades out each time price data refreshes. CSS keyframe animation, `duration: 1.2s`, triggers on data fetch. Do not add any other animations.
- **Price change flash:** when WEN price updates, the number briefly flashes — white → red (if down) or white → slightly brighter white (if up). CSS transition, `200ms`.
- **No scroll animations, no parallax, no reveal effects.**

---

## Tone & Copy Notes

- Write every label in sentence case, not title case
- Never use the word "revolutionary", "ecosystem", "utility", or "community" anywhere
- The site is aware it's buying Wendy's stock with meme coin fees — lean into the deadpan. Treat it exactly like a serious hedge fund would treat any other investment thesis.
- One permitted moment of personality: in the footer disclaimer, add in small muted text — `"We like the burgers."`

---

## API Implementation Notes

```javascript
// Finnhub live price fetch (poll every 15s)
const fetchPrice = async () => {
  const res = await fetch(
    `https://finnhub.io/api/v1/quote?symbol=WEN&token=${API_KEY}`
  );
  const data = await res.json();
  // data.c = current price
  // data.d = change
  // data.dp = % change
  // data.h = high
  // data.l = low
};

// Holdings (update this object manually after each purchase)
const holdings = {
  shares: 0,
  avgCostPerShare: 0.00,
  lastUpdated: "2025-01-01",
  totalInvested: 0.00,
};
```

---

## What to Avoid

- No particle effects or animated backgrounds
- No glowing neon anything
- No rotating logos or entrance animations
- No Solana-purple colour anywhere
- No countdown timers
- No "whitepaper" button linking to a Google Doc
- No Discord widget embedded in the page
- Do not use any stock photo of a Wendy's restaurant or burger

---

*Build as a single self-contained HTML file with all CSS and JS inline unless using React, in which case a single `.jsx` file with Tailwind via CDN.*
