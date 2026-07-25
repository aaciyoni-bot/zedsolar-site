# ZedSolar ☀️🇿🇲

**Solar & backup power, paid for with Mobile Money — made for your phone.**

ZedSolar is a mirror-store for AliExpress **specialised in solar and energy for
Zambia**: solar panels, home lighting kits, portable power stations, inverters,
backup batteries, rechargeable lights, fans, power banks and solar street lights —
the same products, priced in Zambian Kwacha (ZMW) and paid with **MTN MoMo, Airtel
Money or Zamtel Kwacha** — no credit card required. Where the grid is unreliable,
demand is huge and high-ticket items (panels, power stations) mean strong profit per order.

Built on the proven [ZedGlow](https://github.com/aaciyoni-bot/zedglow-site)
architecture, re-themed to the solar/energy niche and tuned mobile-first. An
**ORIZIS TECHNOLOGY** brand.

## Business model

Customer pays with Mobile Money → order lands in WhatsApp (with a direct AliExpress
link per item) → we order the item on AliExpress to the customer's address →
drop-ship (12–25 days).

**Company profit = 30 % markup** applied to every item price (`MARKUP_PERCENT: 30`),
plus a 5 % service fee at checkout that covers Mobile Money charges.

## Structure

Single-page storefront + a small backend.

| File | Purpose |
|------|---------|
| `index.html` | The entire storefront — catalog, cart, Mobile Money checkout. All config lives in the `CONFIG` block at the top of the `<script>`. |
| `backend/` | Express (Vercel serverless): `/api/products` proxies AliExpress via RapidAPI (key server-side), `/api/pay` + `/api/pay/status` drive pawaPay Mobile Money. |
| `manifest.json`, `sw.js`, `icon-*.png` | PWA — installable "app" on Android/iPhone. |

### CONFIG (top of `index.html`)

```js
API_BASE_URL: "https://zedsolar-site.vercel.app",  // backend; "" = demo mode
USD_TO_ZMW: 27.5,          // exchange rate USD → Kwacha
MARKUP_PERCENT: 30,        // company profit on every purchase
SERVICE_FEE_PERCENT: 5,    // checkout fee (covers MoMo charges)
BUDGET_MAX_ZMW: 300,       // ceiling for the "Deals under K300" strip
WHATSAPP_ORDERS: "260971234567",
WHATSAPP_SUPPORT: "260971234567"
```

If the backend is unreachable (or has no `RAPIDAPI_KEY`), the storefront falls back to
a built-in set of **demo solar products** — it never shows a broken page.

## Deploy

**Storefront** (static, no build): push to `main` → GitHub Pages. Optional custom
domain via a `CNAME` file.

**Backend** (Vercel):
- Root Directory: `backend`
- Env vars: `RAPIDAPI_KEY` (required for live products), `PAWAPAY_TOKEN` +
  `PAWAPAY_ENV=production` (required for real Mobile Money charges).
- Health check: `GET /api/health`.

## Going live checklist

1. Add `RAPIDAPI_KEY` in Vercel → real solar catalog replaces the demo products.
2. Create a [pawapay.io](https://pawapay.io) account → set `PAWAPAY_TOKEN`
   (sandbox → production) → real Mobile Money charges.
3. Set your real `WHATSAPP_ORDERS` / `WHATSAPP_SUPPORT` numbers in `CONFIG`.
4. Point a domain (e.g. `www.zedsolar.co.zm`) at GitHub Pages via `CNAME`.
