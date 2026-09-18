# 🚀 انتشار سی‌پی شاپ (CP.Shop) روی اینترنت با دامنه اختصاصی

Complete guide to get your store live on your own domain and indexed by Google.
Time: ~30 minutes active work + up to 24h DNS propagation.
Cost: domain only (~$1–15/year). Hosting is **free** (Netlify).

---

## Part 0 — Before you buy (important for an Iranian game top-up business)

Since the site accepts payments (currently a demo), before taking real orders you need:

| Requirement | Iran equivalent | Where |
|---|---|---|
| Business identity | پروانه کسب / شرکت ثبت‌شده | سامانه ثبت شرکت‌ها |
| E-commerce trust seal | اینماد (enamad.ir) | required by law for Iranian e-commerce |
| Payment gateway | زرین‌پال / آیدی‌پی / به‌پرداخت | needs اینماد for real settlements |
| Domain + host sanctions check | OFAC/general sanctions | most registrars block .ir or Iranian TLDs; see Part 1 |

> ⚠️ If you operate fully inside Iran with an Iranian gateway, you can also host **inside Iran**
> (noاینماد needed for .ir domains via ایرانیک/آروان). The guide below uses the **international free path**.

---

## Part 1 — Buy the domain (~10 min)

You need an international domain because the store is hosted on Netlify/GitHub Pages.

1. Pick a registrar that works from Iran: **Namecheap**, or Iranian sellers of international TLDs
   (ایران سرور، میزبان فا، پارس‌پک sell .com too and accept Toman payment).
2. Choose a short name — suggestions: `cpshop.ir` or `cp-shop.ir` (best match for the brand),
   `cpshop.store`, `cpshop.shop` (premium TLD, usually pricier), `gemshop.ir`.
3. Buy it (Toman payment works on Iranian registrars; Namecheap needs an international card).
4. Leave the registrar tab open — you'll enter DNS records in Part 3.

**Sanctions note:** if the registrar asks and you select Iran, some registrars (Namecheap, GoDaddy)
will refuse the order due to sanctions. Iranian registrars are the workaround and fully legal inside Iran.

---

## Part 2 — Free hosting (Netlify, 5 min)

The site is one static `index.html` + `robots.txt` + `sitemap.xml` — no backend needed yet.

1. Go to **https://app.netlify.com/drop**
2. Sign up free (email or Google — VPN may be needed from Iran).
3. Drag the whole `gametop` folder into the Drop zone.
4. Netlify gives you a temporary URL like `random-name-12345.netlify.app`.
5. Click **Site settings → Change site name** → set it to `cpshop` → URL becomes
   **`https://cpshop.netlify.app`** (free, permanent, Google-indexable even before you connect a domain).

> Alternative: GitHub Pages (free, permanent). Needs a GitHub account + the files in a repo →
> Settings → Pages → deploy from branch. Same result, slightly more steps.

---

## Part 3 — Connect your domain (10 min + waiting)

1. In Netlify: **Site configuration → Domain management → Add a domain** → enter `yourdomain.com`.
2. Netlify shows you DNS records. In your registrar's DNS panel add:
   - **A record:** `@` → `75.2.60.5`
   - **CNAME record:** `www` → `your-site-name.netlify.app`
   (or change the domain's nameservers to Netlify's — simpler, Netlify then manages everything)
3. Wait for propagation: `ping yourdomain.com` from cmd — when it answers, HTTPS
   auto-provisions in Netlify within minutes (Let's Encrypt, free).

---

## Part 4 — Get on Google (15 min + days of patience)

Google indexes automatically eventually, but this forces it fast:

1. Go to **https://search.google.com/search-console** → sign in with a Google account.
2. Add property → **URL prefix** → `https://yourdomain.com`.
3. Verify ownership: choose the **HTML file** method — download the file Google gives you,
   drop it in the `gametop` folder next to index.html, re-drag the folder to Netlify, click Verify.
4. Submit your sitemap: in Search Console left menu → **Sitemaps** → enter `sitemap.xml` → Submit.
5. **URL Inspection** (top bar) → paste `https://yourdomain.com` → click **Request Indexing**.
6. Expect first indexing within a few days; search «سی‌پی شاپ» or your brand after a week.

**SEO already built into the site:** Persian meta description, Open Graph cards for Telegram/WhatsApp,
`robots.txt`, `sitemap.xml`, `OnlineStore` JSON-LD structured data, canonical URL, favicon,
mobile-friendly responsive layout (Google indexes mobile-first).

**One required code change after buying the domain:**
open `index.html` and replace every occurrence of `cpshop.example` with your real domain.
It appears in: canonical link, og:url, JSON-LD `url`, robots.txt sitemap line, sitemap.xml loc.
Without this, Google may index the wrong canonical URL.

---

## Part 5 — When you're ready for real payments

The checkout flow is front-end only. To take real Toman:

1. Get اینماد (enamad.ir) — needs business identity.
2. Register at a gateway (زرین‌پال is easiest) and get a merchant code (مرچنت کد).
3. Replace the simulated gateway step in `renderPayGateway()` in index.html with a redirect to
   `https://www.zarinpal.com/pg/StartPay/{authority}` — this requires a tiny backend
   (Node/PHP) to request payment and verify it; static sites can't hold the merchant secret.
   Ask me and I'll build that backend when you get there.

---

## Quick checklist

- [ ] Domain bought (Part 1)
- [ ] Netlify account + site dropped (Part 2)
- [ ] DNS records added, HTTPS green (Part 3)
- [ ] `cpshop.example` replaced with real domain in 5 places (Part 4)
- [ ] Search Console verified + sitemap submitted + indexing requested (Part 4)
- [ ] اینماد + real gateway before taking real payments (Part 5)
