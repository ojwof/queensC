# Queens Cargo — Project Tracker

> **Context:** The platform is **already built and working**: public website, tracking engine, Express/SQLite API,
> admin console, documents, quotes, package photos and payments. Claude is acting as a **senior professional developer** who
> maintains and improves this existing codebase. Nothing here is built from scratch; every task modifies the working system
> in place and must leave it working. See `CLAUDE.md §0`.
>
> **2026-10-12: Queens Cargo launch.** The site launches as **Queens Cargo** on **queenscargo.com**, with the admin console
> at **`https://queenscargo.com/private-user/`** (a path on the same domain and app, not a subdomain). The step-by-step
> prompts are in `PROMPTS.md`. It is a new site: new GitHub repo with fresh history, new Hostinger Node.js app, new empty
> database, new admin credentials.

**Working method:** treat this like a client engagement on a live system. Before each task, read the affected files; after each task, run `npm run build`, test the flow you touched, commit with a clear message, and log it below.

**Owner:** Queens Cargo · **Repo:** new Queens Cargo GitHub repo, fresh history (L.9) · **Target:** queenscargo.com on Hostinger (new Node.js app)
**Status legend:** `[ ]` to do · `[~]` in progress · `[x]` done · `[!]` blocked (see Needs owner)

---

## Phase L — Queens Cargo launch (current phase; PROMPTS.md P1–P7)
**Confirmed by the owner (2026-10-12):** name, legal name and short name **Queens Cargo** (no apostrophe in text; the logo
artwork reads "QUEEN'S") · tagline **Faster Moves. Bigger Reach.** · **info@queenscargo.com** · **queenscargo.com**
(`www.` redirects to the root) · admin **/private-user/** · tracking ID **`QS` + 6 characters = 8** (e.g. `QS7K2M9P`, piece
labels `QS7K2M9P-01`) · references `QS-TKT-`, `QS-INV-`, `QS-SL-` + 6 digits (quotes keep `QR-YYYY-#####`) · logo source
`images/queenscargo.png` · palette, fonts, layout and photos unchanged · default DB file `queenscargo.db`.

- [x] L.1 Safety net (P1): branch `queenscargo`, launch plan committed, every git remote removed from the local clone, baseline `npm run build` + `npm test`, old-brand inventory (kept in chat only).
- [x] L.2 Logo and icons (P2): `scripts/optimize-images.mjs --brand` from `images/queenscargo.png` → `Public/brand/` (logo, logo-white, mark, favicons, app icons, OG image); cache-bust `?v=6`; size check in header, drawer, footer, admin, documents and tab.
- [x] L.3 Name, domain, email (P3): `src/config/brand.ts`, `index.html` meta/OG/Twitter/JSON-LD, `site.webmanifest`, legal text, documents, server health name and start log, settings defaults, package `queens-cargo`, default DB file `queenscargo.db`, `.env.example`; CLAUDE.md, BRAND_GUIDE, CONTENT.
- [x] L.4 Tracking IDs and references `QS` (P3): 8 characters = `QS` + 6 (`ID_LENGTH` in `src/shared/trackingId.ts`), `QS-TKT/INV/SL`, placeholders, help text, `scripts/*.test.ts`.
- [x] L.5 **Admin console at `/private-user/`** (P4): verified in production mode under the new name; white logo added above the login card; read-only "Admin console address" (`ADMIN_URL`) in Settings → Company & Carrier Identity.
- [x] L.6 Docs (P5): DEPLOYMENT (one domain + `www.` redirect, `/private-user/`, new repo and app, `queenscargo-data/`, smoke test, backups); this tracker rewritten for the new site.
- [x] L.7 Final sweep + full local run (P5): see the launch section of the change log.
- [ ] L.8 New admin password + session secret (P6), new values never used by any other site.
- [ ] L.9 New GitHub repo with fresh history + Hostinger app, env vars, domain, SSL, smoke test (P7 = Phase 7 below).
- [ ] L.10 (optional, after launch) Rename the internal `ngf` code names to `qc` (PROMPTS.md Prompt 9).

---

## Phase 0 — Take over the existing system & record a baseline
- [ ] 0.1 Create a new GitHub repo for Queens Cargo and push the code without the old git history (DEPLOYMENT §2). *Done in L.9 / 7.0.*
- [x] 0.2 Copy `CLAUDE.md` to the repo root and all other docs into `/docs`.
- [x] 0.3 Run `npm install` and `npm run dev`; confirm the existing site, tracking, admin login, shipment creation, documents and quotes all work (this is the baseline that must never regress).
- [x] 0.3b Codebase walkthrough: read App.tsx, the main pages, server routes and db.ts; write a short "system notes" summary at the bottom of this tracker so future sessions start with context.
- [ ] 0.4 Take "before" screenshots of every page → `screens/before/`.
- [x] 0.5 Site images supplied and built into `Public/images/site/` (BRAND_GUIDE §8, CONTENT §12).
- [x] 0.6 Owner supplies logo files (full-colour, white/reversed, icon-only) → `Public/brand/`. *Derived from `images/queenscargo.png` (L.2); a transparent or vector original is still wanted (#60).*

## Phase 1 — Brand foundation (no visual redesign yet)
- [x] 1.1 Create `src/config/brand.ts` with every value from `CLAUDE.md §2`; replace hard-coded brand strings with imports.
- [x] 1.2 Extract the colour palette from the logo (BRAND_GUIDE §4), then write the tokens into `src/styles/tokens.css`.
- [x] 1.3 Brand token prefix `--ngf-*` and class prefix `ngf-` across `src/` (BRAND_GUIDE §4.1).
- [x] 1.4 Replace `logo.png`, footer logo, favicon, apple-touch-icon; add `site.webmanifest` and OG image.
- [x] 1.5 Update `index.html`: title, description, Open Graph/Twitter tags, theme-color, canonical (CONTENT.md §1).
- [x] 1.6 `package.json` name `queens-cargo`; default DB file `queenscargo.db` (server/db.ts, .env.example, DEPLOYMENT §4).
- [x] 1.7 Replace all emails with `info@queenscargo.com` (defaults in `server/db.ts` settings, Header, Contact, PublicQuoteResult, Admin Settings).
- [x] 1.8 Implement the new tracking-ID generator (`QS` + 6 chars = 8 total) as ONE shared util; replace all generators. *The server assigns every ID; clients adopt it. Tests: `npm test`.*
- [x] 1.9 Update tracking input validation, placeholders and help text to the new 8-character format.
- [x] 1.10 Worldwide demo shipments for development *(all demo data removed for launch, 6.7)*.
- [x] 1.11 Replace the other ID prefixes: seals, support tickets, invoices, returns (BRAND_GUIDE §7). *`src/shared/references.ts`.*
- [x] 1.12 Brand sweep: no foreign brand names in source. *Re-run 2026-10-12 for the Queens Cargo launch: zero hits.*

## Phase 2 — Worldwide operations
- [x] 2.1 Replace the U.S. hub data with the global gateway network (CONTENT.md §9) in `HomeNetworkMap`, `FacilityNetworkMap`, `LocationsPage`.
- [x] 2.2 `geocodingService.ts`: add a `GLOBAL_GATEWAY_DATABASE` (city, country, ISO code, lat/lng, IANA time zone); keep Nominatim for free-text; stop assuming `state`/`zip`.
- [x] 2.3 `routingEngine.ts`: when OSRM cannot route (different continents/oceans), draw air/sea legs as great-circle arcs instead of failing.
- [x] 2.4 Rename `USJourneyMap` → `JourneyMap` (component + CSS + imports); remove "interstate/highway" wording.
- [x] 2.5 Time zones: replace `ET/CT/MT/PT` handling (server/routes/track.ts, shipments.ts) with IANA zones + UTC offset display.
- [x] 2.6 Address forms (Quote, Ship, Admin Create Shipment): country selector first, then fields that fit that country (postcode optional, "State/Region").
- [x] 2.7 Units: kg/cm by default with lb/in toggle; currency display configurable (default USD, allow NGN/GBP/EUR).
- [x] 2.8 Service modes: add Air Freight / Ocean Freight / Road labels to shipment types where the copy mentions them.

## Phase 3 — Copy (page by page, from CONTENT.md)
- [x] 3.1 Header + footer + mobile drawer (nav labels, footer columns, legal line, social links)
- [x] 3.2 Home — all 13 sections
- [x] 3.3 Services
- [x] 3.4 About
- [x] 3.5 Locations
- [x] 3.6 Quote + Public Quote Result *Quote reference keeps its `QR-` format (#35); "Estimated transit" hidden until quotes store one (#32).*
- [x] 3.7 Ship *Server-written first tracking event still needs work (#33).*
- [x] 3.8 Track + Track Result + loading screen + support modal *No public Documents panel yet (#28).*
- [x] 3.9 Help / FAQ
- [x] 3.10 Contact
- [x] 3.11 Legal (Privacy, Terms, Cookies, Shipping Terms, Accessibility) — **must be reviewed by a lawyer before launch** *Plain-language drafts in place (CONTENT §13.1); lawyer review still open (#11, #37).*
- [x] 3.12 Generated documents: waybill, BOL, invoice, receipt, label, insurance certificate (ShipmentDocuments, DocumentCenterView, CreateShipmentView) *No Proof of Delivery or Security Seal Record document type yet (#38).*
- [x] 3.13 Admin console strings (AdminLayout, AdminLogin, Settings defaults)
- [x] 3.14 Replace invented testimonials & client logos per CONTENT.md §2.10–2.11 (remove until real ones exist) *"Our commitments" + "Modes we connect" on Home.*

## Phase 4 — Images
- [x] 4.1 Optimise all site images (WebP + JPG fallback, max 2400px wide hero, ≤ 250 KB each where possible).
- [x] 4.2 Swap image paths in Home, Services, TrackResult; delete old image files from `Public/`.
- [ ] 4.3 Write alt text for every image (CONTENT.md §12).

## Phase 5 — 3D & motion (MOTION_3D_SPEC.md)
- [x] 5.0 Code-split first (MOTION_3D_SPEC §2): the admin console, Leaflet and the PDF libraries load only when needed. Measurements below.
- [ ] 5.1 Motion foundation: `useReveal`, `useTilt`, `useParallax` hooks + reduced-motion guard + (optional) Lenis smooth scroll.
- [ ] 5.2 Hero 3D globe with animated trade-route arcs (lazy-loaded; static fallback image).
- [ ] 5.3 Section reveals + staggered cards on all pages.
- [ ] 5.4 3D tilt on service/industry/value cards.
- [ ] 5.5 Parallax depth on image sections and page heroes.
- [ ] 5.6 Animated counters + "How it works" connecting path animation.
- [ ] 5.7 Track Result: tilted map, animated marker, 3D parcel "passport" card flip.
- [ ] 5.8 Performance pass: Lighthouse mobile ≥ 85 Performance, ≥ 95 Accessibility; fix regressions.

### Performance measurements (5.0, 2026-09-29)
Production build served by `node dist-server/server/index.js` on localhost (scratch DB). Lighthouse 12.8.2 CLI, default mobile preset (simulated mid-range phone, slow 4G, 4× CPU slowdown), median of 3 runs.

| Chunk | Before | After | Loaded when |
|---|---|---|---|
| Main JS (every public page) | 1,806 kB (489 kB gzip) | **712 kB (192 kB gzip)** | always (budget ≤ 250 kB gzip: **met**) |
| Admin console | (in main) | 344 kB (77 kB gzip) + login 1.5 kB | only on the admin page |
| Leaflet | (in main) | 150 kB (44 kB gzip) | when a map is within 200 px of the viewport |
| jspdf + html2canvas (+ their deps) | (in main) | 782 kB (242 kB gzip) | only when a document PDF is downloaded |
| CSS | 434.6 kB (71 kB gzip) | 434.6 kB (71 kB gzip), byte-identical | always (see #41) |

| Page | Performance | Accessibility | FCP | LCP | TBT | CLS |
|---|---|---|---|---|---|---|
| Home | 37 → **47** | 95 → 94 | 13.5 → **7.1 s** | 15.9 → **9.4 s** | 1,016 → **452 ms** | 0.028 → 0.028 |
| Track | 48 → **52** | 98 → 98 | 12.5 → **8.0 s** | 13.9 → **9.3 s** | 345 → **306 ms** | 0.009 → 0.010 |
| Services | 51 → **52** | 98 → 98 | 12.5 → **7.5 s** | 14.0 → **9.0 s** | 251 → 291 ms | 0.029 → 0.029 |

These figures predate HTTP compression (added later in `server/index.ts`, see Decisions). Open accessibility items for 5.8: red eyebrow/"read more" text on the dark sections (#D3070B on #181818, 3.21:1) and the barcode spec captions (#64748B on #F1F5F9, 4.34:1) need colour decisions.

## Phase 6 — QA & SEO
- [x] 6.1 Brand sweep = zero. *Re-run 2026-10-12 for the Queens Cargo launch over source, docs, `dist/` and `dist-server/`.*
- [x] 6.2 Functional test: create shipment in admin → track it publicly → documents download → quote flow → contact form. *Re-run end to end for the launch (L.7).*
- [!] 6.3 Cross-browser: Chrome, Safari (iOS), Firefox, Samsung Internet; low-end Android test. *Needs the owner's devices (#43). Desktop Chrome checked at 375, 768 and 1440.*
- [x] 6.4 SEO: per-page titles/descriptions, `robots.txt`, `sitemap.xml`, OG image, structured data (Organization). *`server/seo.ts`: one public robots.txt/sitemap on `https://queenscargo.com` that never lists the admin path, plus `X-Robots-Tag: noindex, nofollow` on `/private-user/`. Hash routing limits indexing to `/` (#44).* **Before announcing the launch:** the OG image URL is absolute (`https://queenscargo.com/brand/og-image.jpg`) and only works once the domain serves the site; check it with DEPLOYMENT §7.
- [x] 6.5 Accessibility: keyboard nav, focus states, alt text, contrast, reduced motion. *axe-core (WCAG 2.1 AA) on 11 public pages at 375 and 1440: 0 violations. Not audited: the admin console and later form steps (#45).*
- [ ] 6.6 Take "after" screenshots → `screens/after/`.
- [x] 6.7 **Remove ALL demo data before launch (launch blocker).** No mock shipments, demo seed, fake timeline or demo map dot in the code; `SEED_DEMO_DATA` no longer does anything; the production database starts empty. `grep -rn "DEMO DATA" src server` returns nothing.

## Phase 7 — Deploy Queens Cargo (DEPLOYMENT.md, PROMPTS.md P7)
- [ ] 7.0 New, empty GitHub repo for Queens Cargo; push a **fresh-history** commit (orphan branch → `main`). `git remote -v` must show only the Queens Cargo repo.
- [ ] 7.1 **New** Hostinger Node.js app connected to the Queens Cargo repo (`main`); Node 22.x (≥ 22.5); install `npm install` (postinstall builds), start `npm start`.
- [ ] 7.2 Env vars: `NODE_ENV=production`, new `ADMIN_PASSWORD_HASH` + `SESSION_SECRET` (L.8), `DB_PATH=/home/<user>/queenscargo-data/queenscargo.db`, `UPLOADS_DIR=/home/<user>/queenscargo-data/uploads` (outside the deploy folder), `SEED_DEMO_DATA` unset.
- [ ] 7.3 Domain: `queenscargo.com` on the app, `www.` redirecting to the root (one host keeps the admin session working); SSL on both, force HTTPS. **No admin subdomain**: the admin is `https://queenscargo.com/private-user/`.
- [ ] 7.4 Email: info@queenscargo.com mailbox + SPF/DKIM/DMARC.
- [ ] 7.5 Post-deploy smoke test (DEPLOYMENT §7): logo/name/tab icon, OG preview, `/private-user/` login with the new password, `/#/admin` shows Home and `/admin` the 404 page, robots.txt doesn't list the admin path, `/api/diag/storage` shows `queenscargo-data`, create → track a QS shipment, data survives a redeploy, compression + `/assets` cache headers. Then set up the daily backup of `queenscargo.db` **and** the `UPLOADS_DIR` folder.

---

## Blocked / Needs owner
Only open items are listed. Numbers are kept stable because code and docs refer to them.

| # | Item | Needed for |
|---|---|---|
| 2 | **Phone number(s) + WhatsApp number.** `PHONE` and `WHATSAPP` in `brand.ts` are empty, so the UI hides them. Never invent them (PROMPTS.md Prompt O). | 1.1, 3.1, 3.10 |
| 3 | **Head-office address** (and any regional offices). `HQ_ADDRESS` is empty and hidden. | 1.1, 3.10, 6.4 |
| 4 | Confirm the global gateway list (CONTENT.md §9), live on Home, Locations and Contact from `src/data/gateways.ts`. Includes the Tokyo (NRT), Seoul (ICN), Chicago (ORD) and Miami (MIA) gateways and their lanes NRT–LAX, ICN–FRA, ORD–LHR, ORD–PVG, MIA–GRU. | 2.1 |
| 5 | Year founded / real stats (shipments, countries, years), or approve keeping the stat blocks removed. Home shows only 24/7, "1 tracking ID", "5 continents" and modes. | 3.2, 3.4 |
| 6 | Real customer testimonials + permission (Option B, CONTENT §2.10). Home shows "Our commitments". | 3.14 |
| 7 | Real partner/carrier logos you're authorised to show. Home shows "Modes we connect". | 3.14 |
| 8 | Certifications/licences you actually hold (e.g. IATA, customs broker licence, ISO). | 3.2, 3.4 |
| 9 | **Social media links.** `SOCIAL` in `brand.ts` is empty, so no social icons show. | 3.1 |
| 11 | **Lawyer review of legal pages** (all five tabs, CONTENT §13.1). Drafts are live on `#/legal`; see #37 for the open facts. | 3.11 |
| 12 | After a control action these stay browser-only: `statusMessage`, the audit log and the hold "frozen" flag (no columns). Decide whether to persist them. | 1.8 |
| 14 | No email notification is sent when a contact message, callback, support ticket or payment proof arrives (needs an SMTP/email provider). Covers #21 and #30. | 2.1, 3.10 |
| 15 | Sea legs are great-circle arcs, which cross land on some lanes (e.g. Shanghai → Rotterdam over Siberia). Decide whether to add real sea lanes (e.g. the MIT-licensed `searoute` data). | 2.3 |
| 17 | **Exchange rates are entered by hand** in Settings → Tariff. Decide who updates them and how often, or connect a rate feed. | 2.7 |
| 18 | The automatic estimates (Quote page tariff corridor, admin default base rates) are placeholder USD formulas; they need real pricing. The public quote page invents a breakdown line when a quote is published with only a total. | 2.7, 3.x |
| 19 | Still imperial by nature: NMFC freight classes, "48×40 in" pallet names, IATA kennel sizes in inches, dimensional-weight divisor 139. Confirm whether to add metric equivalents. | 2.7 |
| 20 | Footer: **Careers** hidden until a careers page exists; no newsletter (needs a mailing-list provider and copy). The admin "regulatory line" shows on About and Contact when set, not in the header. Confirm. | 3.1, 3.11 |
| 22 | Most Home photo sources are ~347 px squares, upscaled on wide screens. Larger originals sharpen them with no code change. | 3.2, 4.2 |
| 24 | **[confirm] items on Services/About:** Priority Express max 70 kg per piece and 120 × 80 × 80 cm; vehicle types; Secure Vault max 30 kg per piece; Weekend Delivery "on selected lanes" (which?); comparison-table Transit speed / Modes values (CONTENT §3.2); same-day collection cut-off; Services eyebrows and About "WHAT WE'VE BUILT" header. | 3.3, 3.4 |
| 25 | About timeline cards show no years until real dates are supplied (`MILESTONES` in `AboutPage.tsx`). | 3.4 |
| 27 | The admin pre-filled customer message per status (TrackingEventsView) is not in CONTENT. | 3.8, 3.13 |
| 28 | **Track Result: no Documents panel yet.** Needs a decision on which documents the public may download. | 3.8, 3.12 |
| 31 | **Quote, Quote Result, Ship, Help and Contact labels** recorded in CONTENT §7.4/§8.4 (e.g. `Drop-off at a Queens Cargo gateway`, `Book shipment`, `Print labels`): confirm or replace. | 3.6, 3.7, 3.9, 3.10 |
| 32 | **Quote Result:** no transit time stored, so "Estimated transit" is hidden; the publish form sets one total, so the breakdown shows one Transport line; "Download PDF" opens the print dialog; no rate email is sent (#14). | 3.6 |
| 33 | **Ship booking:** the server's first tracking event for every new shipment reads "Consignment Registered & Barcode Issued … Linear Code 128 barcode assigned"; the Ship page sends a fixed ETA "2-3 Business Days, by 5:00 PM" and facility "{city} Regional Gateway". Confirm the three collection windows and whether Queens Cargo accepts drop-off at a gateway. | 3.7, 3.12 |
| 34 | **Help [confirm] answers:** same-day cut-off, damage reports "within 7 days" (CONTENT §8.2). Help, Contact and Quote heroes use Unsplash photos. | 3.9, 3.10, 4.2 |
| 35 | **Contact / Quote reference:** "Average response under 30 minutes" is confirmed for callbacks only; confirm for messages. Quotes keep `QR-YYYY-#####` (the quote link and tracking search depend on `QR`); decide whether quotes get a `QS-` reference format. | 3.6, 3.10 |
| 37 | **Legal facts for the owner and the lawyer:** {{JURISDICTION}} (governing law names "the country in which Queens Cargo is registered" until known; TODO in `src/data/legalDocs.ts`); dimensional-weight divisor 5000 and 7-day claims window [confirm]; data-retention periods; liability limits; prohibited/restricted items; "we do not sell personal data"; the Cookie Policy lists the real cookie (`ngf.sid`, staff only), the storage keys and third-party services (Esri tiles, Google Fonts, Unsplash, Nominatim, OSRM). | 3.11 |
| 38 | **Documents:** no Proof of Delivery or Security Seal Record type yet; Shipping Label, Shipment Receipt and Certificate of Cargo Insurance titles to confirm (CONTENT §11); a shipment created without dimensions stores 12 × 12 × 12 in, which prints on its documents; the PDF is an image (text not selectable). | 3.12 |
| 39 | **Admin Settings** still shows the U.S. "USDOT & Carrier Authority Number" field with an FMCSA hint. Rename it once you say which registration Queens Cargo shows. `defaultFuelSurchargeRate` 8.5% is in no doc: confirm or clear. | 3.13 |
| 41 | Public pages depend on some admin stylesheet rules, so the admin CSS stays in the main bundle. Follow-up (no visual change, needs its own parity check): move the shared rules to public styles and drop ~29 kB gzip of admin CSS for visitors. No owner decision needed. | 5.8 |
| 42 | Quote conversion writes the converted shipment's first event with a `350.00` tariff fallback and the "Linear Code 128" wording (same as #33). | 6.2 |
| 43 | **Manual device checks** on your phone (iPhone Safari, Android Chrome / Samsung Internet): tracking, Ship booking, country and phone pickers, mobile menu, maps, PDF/print, Contact and Callback. | 6.3 |
| 44 | **Search engines can index only the home page** (hash URLs). Indexable page URLs need path-based routing. Decide before or after launch. | 6.4 |
| 45 | **Accessibility not yet covered:** the admin console, later Quote/Ship form steps and modal states; modals close with Escape but don't trap focus. | 6.5, 5.x |
| 46 | Track Result "Get Delivery Alerts" form says "Subscribed Successfully!" but saves nothing. Nothing opens it (dead code): delete it or connect it to email/SMS (#14). | 6.2 |
| 47 | **Found in QA, not fixed:** (a) the dangerous-goods **Emergency Response Contact** field is not saved; (b) the admin "Book Consignment" quick form is U.S.-shaped (required "State", NY/LA placeholders, lbs only); (c) the pet presets pre-fill a fictional "(212) 555-0199"; (e) `POST /api/quotes` accepts a client-chosen quote `id`, and `POST /api/documents/generate` (public, rate-limited) returns 500 for a bad body and accepts an unknown `docType`. | 6.2 |
| 50 | At 375 px the footer tagline may wrap onto two lines (no overflow). | 3.1 |
| 52 | At 375 px the Quote result page (`#/quote/<id>`) scrolls sideways by about 40 px (`.quote-main-col` / `.pending-rate-waiting-card`). | 6.2 |
| 55 | **Launch inputs still needed:** (a) the URL of the new, empty Queens Cargo GitHub repo (L.9); (b) the Hostinger plan/app and access to the queenscargo.com DNS; (c) the info@queenscargo.com mailbox (7.4); (d) phone / WhatsApp / HQ address / social links (#2, #3, #9); (e) lawyer review under the Queens Cargo name and jurisdiction (#11, #37); (f) confirm the payment/proof copy in CONTENT §6.8 and the 404 page copy in CONTENT §1.1a. | L, 7 |
| 57 | The Document Center preview closes on a backdrop click but not with Escape. The ETA engine gives an Express Tokyo → São Paulo shipment next-day delivery: confirm the transit rules. | 3.12 |
| 59 | **Code clean-up after launch (owner to decide):** (a) the unused `ADMIN_PROXY_TARGET` proxy mode in `server/index.ts`, `server/db.ts` and `server/seo.ts` (DEPLOYMENT §4: leave it unset); (b) remove `/api/diag/storage` once persistence is confirmed on Hostinger; (c) `QuoteRequestsView.tsx` still gives quote ID `QR-2026-00124` a "SAMPLE" row style (styling only, no data). | 7, after launch |
| 60 | **Queens Cargo logo source is a raster with a checkerboard baked in.** Keyed out automatically; the thin white letter outlines are dropped, and a 1 px pinkish taper remains where the crown meets its band (visible only zoomed in on dark). The tagline is ~2 px tall at header size. A transparent or vector original would give cleaner edges and a crisper 16 px favicon. | L.2 |

## Decisions log
| Date | Decision |
|---|---|
| 2026-10-12 | **Queens Cargo launch (owner):** Queens Cargo (name, legal name and short name; no apostrophe in text); tagline "Faster Moves. Bigger Reach."; queenscargo.com; info@queenscargo.com; tracking ID `QS` + 6 characters = 8; references `QS-TKT/INV/SL-######`; palette, fonts, layout and photos unchanged; new repo with fresh history, new Hostinger app, new empty DB `queenscargo.db`, new admin credentials. |
| 2026-10-12 | **Admin console is a path:** `https://queenscargo.com/private-user/` on the same host and app. `isAdminPath()` matches the path exactly; `#/admin` works only on localhost. The server serves `index.html` there with `X-Robots-Tag: noindex, nofollow`; robots.txt does **not** list the path. The host-only session cookie is also sent on public pages of the same domain (httpOnly + SameSite=lax; the API checks the session on every admin route). `www.` redirects to the root so there is one cookie host. |
| 2026-10-12 | Unknown paths (incl. `/admin`, `/private-users`, `/private-user/x`) get status 404 and the "Page not found" page (CONTENT §1.1a). |
| 2026-10-12 | Admin login shows the white logo (transparent PNG) on the dark photo above the card; Settings shows the admin console address read-only (owner). |
| 2026-10-12 | Internal code names `--ngf-*`, `ngf-`, `ngf_*` and the cookie `ngf.sid` are **kept** for launch (invisible to users; the cookie lives in `server/middleware/auth.ts`). Optional rename after launch: L.10. |
| 2026-10-11 | Production responses are compressed (`compression`); `/assets/*` is cached for a year (`immutable`), `index.html` is `no-cache`, other static files for 1 hour. `@types/qrcode` is a runtime dependency because Hostinger installs with `NODE_ENV=production`. |
| 2026-10-11 | Package images, payment requests and proof of payment: design and decisions D-1…D-9 in `docs/update.md`. |
| 2026-09-30 | No demo data ships: mock shipments, seed, fake timeline and demo map dot removed (6.7). |
| 2026-09-29 | Code-split before motion (MOTION_3D_SPEC §2): admin console `React.lazy`; Leaflet loads when a map nears the viewport; jspdf/html2canvas load when a PDF is generated. Admin CSS stays in the main bundle so the cascade is unchanged (#41). |
| 2026-09-29 | Shipment status names (CONTENT §6.3) live in `src/shared/shipmentStatus.ts`, shared by public pages, admin and server. Stored codes are not renamed: BOOKED = Booked, RECEIVED = Collected, PROCESSING = At origin gateway, DEPARTED_FACILITY = Departed, IN_TRANSIT = In transit, DESTINATION_PROCESSING = Arrived at destination gateway, CUSTOMS_CLEARANCE = Customs clearance, OUT_FOR_DELIVERY, DELIVERED, ON_HOLD, DELAYED, RETURNED = Returning to sender. With PII masking on, the public API masks the signer's name. |
| 2026-09-28 | Returns are real shipments: each return gets its own shipment row and server-assigned tracking ID, linked both ways (`return_leg_json`, `return_of_tracking`). One return per shipment. |
| 2026-09-28 | Transport mode stored per shipment when known; older rows infer it. Air/sea legs are great-circle arcs (#15). |
| 2026-09-28 | Event times: the instant + the IANA zone of the event's location are stored; display is local time there + UTC offset. |
| 2026-09-28 | One canonical unit per quantity in the DB: pounds and inches (columns like `weight_lbs` are not renamed), USD for money. Forms and displays convert at the edges (kg/cm default, lb/in toggle; display currency from Settings at an admin-entered rate). |
| 2026-09-28 | Admin UI shows "Administrator"; new records store it; older "Super Admin" values are shown via `displayOperator`. |
| 2026-09-27 | The server assigns every tracking ID: `POST /api/shipments` and quote conversion ignore any `trackingNumber` in the body. If the server fails, nothing is added locally and an error is shown. |
| 2026-09-27 | Public `/api/track` only accepts IDs in the current format (`TRACKING_PREFIX` + 6 characters; 400 for anything else). |
| 2026-09-26 | Piece labels use one format for every piece type: base ID + `-NN` (e.g. `QS7K2M9P-01`). A search for a piece label resolves to its parent. |
| 2026-09-26 | Keep the four service lines (Priority Express, Scheduled Linehaul/Freight, Vehicle Transport, Secure Vault). |
| 2026-09-26 | Palette from the logo: Primary = graphite, Accent = brand red (500 = `#D3070B`), Ink 950 = `#141414`; Tailwind blues stay as the semantic info colour; in transit = info (blue), delivered = success (green). |
| 2026-09-26 | All pages carry the brand, including admin and generated documents. |
| 2026-09-26 | Admin runs on the same app, domain and API as the public site. The separate `ADMIN_PROXY_TARGET` mode is not used (#59). |
| 2026-09-26 | Owner's source images are in `images/` at the project root and processed into `Public/` by `scripts/optimize-images.mjs`; licence records in `images/*/SOURCES.md`. Every card photo is a 1:1 square. Branded or unverified photos were replaced with free stock; each hero slot has its own photo. |
| 2026-09-26 | Logo: transparent, white and icon versions are derived by `scripts/optimize-images.mjs --brand` until vector files are supplied (source `images/queenscargo.png`). |
| 2026-09-26 | Public contact values resolve as: admin Settings (DB) → `src/config/brand.ts` → empty. Empty phone / address / social / licence values hide their element (`src/utils/useCompanyContact.ts`). |
| 2026-09-26 | `DB_PATH`, `UPLOADS_DIR` and `/api/diag/storage` (admin-only) let production keep data outside the deploy folder (DEPLOYMENT §4). |

## Change log
| Date | Task | Note |
|---|---|---|
| 2026-10-12 | **Queens Cargo launch** | See below. |

### 2026-10-12 Queens Cargo launch
- **P1 (L.1):** branch `queenscargo`; launch plan (`PROMPTS.md`) and the new logo source `images/queenscargo.png` committed; every git remote removed from the local clone (`git remote -v` is empty); baseline `npm run build` and `npm test` recorded; old-brand inventory kept in chat only.
- **P2 (L.2):** logo and icons from `images/queenscargo.png` (2030×775 RGB with a checkerboard baked in) via `scripts/optimize-images.mjs --brand`: checkerboard keyed out by an edge flood fill plus enclosed-pocket and outline-remnant removal, cut edges un-blended from white; `logo.png`, `logo-white.png`, `mark.png` (globe + orbit arrow + parcel), favicons, app icons and OG image; cache-bust `?v=6`. Checked at 2× on white and #181818, in the header, drawer, footer, admin sidebar, documents and tab at 375 and 1440; no size changes needed. New: #60.
- **P3 (L.3, L.4):** `brand.ts` values; tracking IDs `QS` + 6 = 8 (`ID_LENGTH`), references `QS-TKT/INV/SL`; placeholders, help text, `index.html` meta/OG/Twitter/JSON-LD, `site.webmanifest`, package `queens-cargo` (lockfile regenerated), default DB `queenscargo.db`, `.env.example`, tests; CLAUDE.md, BRAND_GUIDE, CONTENT.
- **P4 (L.5):** production-mode check (`NODE_ENV=production`, scratch `DB_PATH`/`UPLOADS_DIR`): `/private-user` 301 → `/private-user/` login; `/private-user-x`, `/private-users`, `/private-user/x`, `/admin` → 404 page; wrong password 401; login OK; created `QS2R9GW7` → "View Shipment Record" opens `/#/track/QS2R9GW7` on the same host; on a production hostname `/#/admin` shows Home; `X-Robots-Tag: noindex, nofollow` on `/private-user/`; robots.txt and sitemap.xml on `https://queenscargo.com` without the admin path; public pages unchanged apart from name and logo. Owner follow-ups: white logo above the login card; read-only "Admin console address" in Settings (CONTENT §10).
- **P5 (L.6, L.7):** DEPLOYMENT.md for the new site; `docs/update.md` kept (it is the payments/photos spec cited by code comments; its test-run records made prefix-neutral) and `docs/PROMPT-UPDATE.md` kept (that feature's prompt, already Queens Cargo); this tracker rewritten for the new site.
  - **Sweep** after a fresh `npm run build` (0 TS errors; the usual >500 kB chunk warning), case-insensitive, for every earlier brand name, domain, email, ID prefix, DB/package name and repo name over src, server, scripts, Public, images, index.html, docs, CLAUDE.md, PROMPTS.md, package.json, package-lock.json, .env.example, `dist/` and `dist-server/`: **0 hits in text and file names**. The only byte matches are 7 photos (and their `dist/` copies) where three random bytes of compressed pixel data happen to spell the old 3-letter prefix in mixed case; none of the 174 images in `Public/` has EXIF, XMP, IPTC or PNG text metadata. Kept on purpose: the internal `ngf` code names (Decisions).
  - **Full run**, production build (`node dist-server/server/index.js`, `NODE_ENV=production`), scratch `DB_PATH` + `UPLOADS_DIR`, temporary password and session secret in the process env only (`.env` and `data/` untouched), headless Chrome: **72/73 PASS**. Login at `/private-user/` (white logo, wrong password rejected) → shipment created over the admin API → `QS` 8-character ID tracks publicly (also lower case with a space and as `-01`) → status In transit shown publicly → all five document types generated in the Document Center UI and downloaded as PDFs (label, receipt, invoice, BOL, insurance: Queens Cargo logo and name, info@queenscargo.com, the QS ID, `QS-INV-` on the invoice) → quote request (public API) → published in the admin UI → public quote page shows $480.00 → Contact form → `QS-TKT-` reference → in the Messages inbox → package photo + payment request (admin API) → proof uploaded on the track page → approved from the dashboard panel → "Paid" / "Payment received" on the track page; Settings shows `https://queenscargo.com/private-user/`. Admin views and 12 public pages at 375, 768 and 1440: Queens Cargo title and logo, no old-brand text, no broken images. **The one miss is the known #52** (Quote result page 38 px sideways scroll at 375). Console: only the expected 401 from the logged-out session check. `npm test` 60/60.
  - **Visual parity:** since the last pre-rename commit the only CSS change is the login logo rule; every other CSS diff is a comment header. Screenshots checked by eye.
  - **No demo data:** no seed or mock files, no `DEMO DATA` markers, `INITIAL_SHIPMENTS` is empty, the only shipment inserts are admin create, quote conversion and returns; nothing reads `SEED_DEMO_DATA`, it is unset in `.env` and `false` in `.env.example`.

---

## System notes (codebase walkthrough)

**Shape.** One Node process. `server/index.ts` (Express 5) serves `/api/*` and the built SPA from `dist/`. Dev: Vite on :3000 proxies `/api` to Express on :5000. Build: `tsc` (src) → `vite build` → `tsc -p tsconfig.server.json` → `dist-server/`.

**Routing.** No router library. `src/App.tsx` keeps `currentPage` in state and syncs it with `location.hash` (`#/services`, `#/track/:id`, `#/quote/:id`). `getInitialPage()` resolves the first paint synchronously; a `hashchange` effect handles the rest. `KNOWN_PAGES` is the allow-list. Admin: `isAdminPath()` (path `/private-user/`, exact match) → always `admin`. On localhost, `#/admin` also opens admin; anywhere else `#/admin` falls back to Home. Any other path than `/` is unknown → `not-found` (NotFoundPage); the server answers those with status 404. The admin console code is a lazy-loaded chunk.

**Frontend ↔ API.** `src/services/api.ts` holds relative `fetch('/api/...')` calls. `AdminDataProvider` (`src/context/AdminDataContext.tsx`) wraps the whole app but loads admin lists only with an admin session. Public tracking: quote IDs (`QR…`) first, then `GET /api/track/:id` (PII-masked). The public site reads company contact info from `GET /api/settings`. `simulationEngine` broadcasts across tabs through the localStorage key `ngf_live_shipment_stream`.

**Server progress.** `server/progress.ts` advances progress and the map position on every read. It imports `src/services/{routingEngine,planningEngine,geocodingService}.ts`, so those files are **shared with Node** and must stay DOM-free.

**Tracking IDs.** Allocated only by `server/trackingIds.ts` via `src/shared/trackingId.ts` (`crypto.randomInt`); clients adopt the server's ID. Piece labels are `QSxxxxxx-NN`; `/api/track` normalises input (case, spaces, dashes) and resolves piece labels to their parent.

**Admin auth.** A single shared password, checked with `bcrypt.compare` against `ADMIN_PASSWORD_HASH` (rate-limited to 20 per 15 min). It sets `req.session.isAdmin`. Session store is in memory: cookie `ngf.sid`, httpOnly, sameSite=lax, secure in production, 12 h, trust proxy 1. **Every restart or redeploy logs everyone out.** `requireAdminAuth` is a plain session check; CSRF protection relies on SameSite=lax.

**DB.** `node:sqlite` (Node ≥ 22.5) at `DB_PATH`, or `<cwd>/data/queenscargo.db` if unset. WAL mode, foreign keys on. The schema is created in `initDatabase()` with try/catch `ALTER` migrations (incl. `deleted_at_ts` for soft delete). Shipment delete is soft (trash); `/permanent` hard-deletes from the trash. Uploaded photos and proofs live in `UPLOADS_DIR` (`images/`, `proofs/`).

**Fragile / worth knowing**
- `/api/diag/storage` is admin-only and returns the server's data paths. Remove it after the Hostinger persistence check (#59).
- **`GET /api/settings` is public** and returns the whole settings object, including `signatureStampUrl`, `signatoryName`, `defaultFuelSurchargeRate` and `hubSortStatus` once set. Consider returning only the public fields.
- The session cookie name (`SESSION_COOKIE`), the logout `clearCookie` and the `ngf_live_shipment_stream` key must change together. Renaming them logs everyone out once.
- Some `'New York'` and `'August 24, 2026'` fallback defaults remain in the server insert path (`server/routes/shipments.ts`) for bodies that omit them.
- Schema columns (`*_state`, `*_zip`, `*_lbs`) are U.S.-shaped. They stay (rule 10); the worldwide work maps onto them.
- `CreateShipmentView.tsx` (≈4,000 lines) and `DocumentCenterView.tsx` (≈2,000 lines): edit surgically.
