# Deployment: GitHub → Hostinger (Queens Cargo)

> **Context:** The Queens Cargo platform is **already built and working**: public website, tracking engine, Express/SQLite API,
> admin console, documents, quotes, package photos and payments. Claude is acting as a **senior professional developer** who
> maintains and improves this existing codebase. Nothing here is built from scratch; every task modifies the working system
> in place and must leave it working. See `CLAUDE.md §0`.

> ⛔ **This is a new site.** Queens Cargo gets a **new GitHub repo** (fresh history), a **new Hostinger Node.js app**, a
> **new, empty database** and **new admin credentials**. Don't connect it to, push to or reuse any other repo, app, domain
> or database.

The app is **one Node.js process** on **one domain**: Express serves the API (`/api/*`), the built React site (`dist/`) and
the admin console at **`https://queenscargo.com/private-user/`**. It stores data in a **SQLite file** plus an
**uploads folder**. That shapes everything below.

---

## 1. Hosting requirements

| Need | Why |
|---|---|
| A Hostinger plan that runs **Node.js web apps** (Business / Cloud web hosting, or a VPS) | Plain static hosting cannot run the Express server, tracking API or admin |
| **Node.js ≥ 22.5** (choose 22.x LTS or newer) | `server/db.ts` uses the built-in `node:sqlite`; `package.json` declares `"engines": { "node": ">=22.5" }` |
| **Persistent storage** outside the deploy folder | The database and uploaded files must survive every redeploy |
| HTTPS (free SSL in hPanel) | The admin session cookie is `secure` in production |

Hostinger's hPanel screens change from time to time. If a label below doesn't match, look for the equivalent setting.

## 2. Repository setup (new repo, fresh history)

1. Create a **new, empty private GitHub repo** for Queens Cargo.
2. Check `.gitignore` still excludes `node_modules/`, `dist/`, `dist-server/`, `/data/` and `.env`.
3. Push one fresh-history commit (none of the earlier history):
   ```bash
   git checkout --orphan queenscargo-main
   git add -A
   git commit -m "Queens Cargo: initial release"
   git branch -M main
   git remote add origin <NEW-EMPTY-QUEENS-CARGO-REPO-URL>
   git remote -v        # must show ONLY the new Queens Cargo repo
   git push -u origin main
   ```

## 3. Hostinger app configuration (new Node.js app)

| Setting | Value |
|---|---|
| Source | GitHub → the new Queens Cargo repo, branch `main` |
| Node version | 22.x (≥ 22.5) or newer LTS |
| Install command | `npm install` (the `postinstall` script already runs `npm run build`) |
| Build command | Leave empty if the panel allows; otherwise `npm run build`. Don't build twice |
| Start command | `npm start` → `node dist-server/server/index.js` |
| Port | Use the port the platform provides (`PORT` env var). The server reads `process.env.PORT` |

## 4. Environment variables (set in hPanel, never commit)

Create the persistent folder first, **outside** the app/deploy directory, e.g. `/home/<hostinger-user>/queenscargo-data/`.

```ini
NODE_ENV=production
PORT=<only if hPanel requires it>
ADMIN_PASSWORD_HASH=<bcrypt hash: see below; new for Queens Cargo, never reused>
SESSION_SECRET=<64 hex chars: see below; new for Queens Cargo, never reused>
DB_PATH=/home/<hostinger-user>/queenscargo-data/queenscargo.db
UPLOADS_DIR=/home/<hostinger-user>/queenscargo-data/uploads
```

Leave these **unset**:
- `SEED_DEMO_DATA`: no longer read; the app ships no demo data (tracker 6.7).
- `ADMIN_PROXY_TARGET`: an admin-only proxy mode that Queens Cargo doesn't use (the admin is a path on this app).
  Setting it would turn this app into an API proxy with no database.

Generate the secrets locally (run them yourself; never paste the password anywhere else):
```bash
node -e "console.log(require('bcryptjs').hashSync('YOUR-STRONG-PASSWORD', 12))"
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

**Database file:** when `DB_PATH` is unset the default is `data/queenscargo.db` (relative to the working directory), which is
fine locally but **inside the deploy folder** on Hostinger, so a redeploy can delete it. The site starts with a fresh,
empty database; the schema and default settings are created on first start. Table and column names are unchanged.

**Uploads folder:** `UPLOADS_DIR` holds package photos (`images/`, shown on the track page) and payment-proof
screenshots (`proofs/`, private, admin-only). Both subfolders are created on start. When unset it defaults to an
`uploads` folder next to the database. Like `DB_PATH`, it **must** point outside the deploy directory.

The startup log (hPanel → app logs) prints the database path and the uploads path in use.

**Check persistence before launch:** `GET /api/diag/storage` is a temporary, admin-only endpoint. Log in at
`/private-user/` first, then open `https://queenscargo.com/api/diag/storage` in the same browser: `dataDir` must be
the `queenscargo-data` folder. Redeploy and open it again; if `markerFirstSeen` stays the same, storage persists. **After
confirming, remove this endpoint.**

## 5. Domain, DNS and the admin path

| Host | Points to | Purpose |
|---|---|---|
| `queenscargo.com` | The Queens Cargo Node app | Public site **and** admin console (`/private-user/`) |
| `www.queenscargo.com` | Redirect (301) → `https://queenscargo.com` | One host only |

- **Admin console: `https://queenscargo.com/private-user/`** on the same host and Node app. There is **no admin
  subdomain** (no `private.` host, no second app). `isAdminPath()` in `src/App.tsx` matches the pathname exactly against
  `ADMIN_PATH` from `src/config/brand.ts` (case and trailing slash ignored; `/private-user/x` and `/private-users` don't
  match). The server answers `/private-user` with a 301 to `/private-user/` and serves `index.html` there with
  `Cache-Control: no-cache` and `X-Robots-Tag: noindex, nofollow`.
- Keep **one** host: `www.` must redirect to the apex, or the host-only admin session cookie splits in two and the
  canonical/OG URLs (which use `https://queenscargo.com`) stop matching.
- On the live site `/#/admin` falls back to Home; `/admin` and any other unknown path get status 404 and the
  "Page not found" page. `/private-user/` is never listed in `robots.txt` or `sitemap.xml` (listing it would advertise it).
- If the domain is registered elsewhere, point its DNS at Hostinger (A/CNAME records from hPanel).
- Turn on SSL for both hostnames, and force HTTPS.

## 6. Email (info@queenscargo.com)

- Create the mailbox (Hostinger Email or Google Workspace).
- DNS: **MX** records from the provider · **SPF** `v=spf1 include:<provider> ~all` · **DKIM** (from the provider) ·
  **DMARC** `v=DMARC1; p=quarantine; rua=mailto:info@queenscargo.com`.
- Send a test to Gmail and check "Show original" → SPF/DKIM/DMARC = PASS.

## 7. Launch checklist (smoke test on the live domain)

- [ ] **No demo data (tracker 6.7).** The production database starts empty; `grep -rn "DEMO DATA" src server` returns
  nothing. Tracking any made-up ID (e.g. `QS7K2M9P`) on the live site says "not found".
- [ ] Home loads over HTTPS with the **Queens Cargo logo, name and tab icon**, and no console errors.
- [ ] `https://www.queenscargo.com/` redirects to `https://queenscargo.com/`; `http://` redirects to `https://`.
- [ ] `/api/health` returns `ok` with `"service": "Queens Cargo API"`.
- [ ] **`https://queenscargo.com/private-user/`** shows the admin login, and the new password works; logout/login works.
- [ ] `https://queenscargo.com/private-user` (no slash) redirects 301 to `/private-user/`.
- [ ] `/#/admin` shows **Home**; `/admin` shows the **404 page**. Neither opens the admin.
- [ ] Response headers on `/private-user/` include `X-Robots-Tag: noindex, nofollow`.
- [ ] `robots.txt` and `sitemap.xml` use `https://queenscargo.com` and **don't mention `private-user`**.
- [ ] Logged in, `/api/diag/storage` shows `dataDir` inside `queenscargo-data`; the startup log shows `UPLOADS_DIR` there too.
- [ ] Create a shipment in admin → it gets an **8-character ID: `QS` + 6 characters** (e.g. `QS7K2M9P`) → the public
  Track page finds it, also typed in lower case with spaces and as a piece label (`…-01`).
- [ ] Download the waybill, BOL and invoice PDFs: Queens Cargo logo, name, `info@queenscargo.com`, `queenscargo.com` and
  `QS-INV-` references only.
- [ ] Quote request → appears in admin → publish → the public quote link opens.
- [ ] Contact and callback forms submit, show a `QS-TKT-` reference and appear in the admin Messages inbox.
- [ ] Package photo upload shows on the track page; payment request → proof upload → approve works.
- [ ] Redeploy once, then confirm the shipment and its photo still exist (persistence).
- [ ] Response headers: `Content-Encoding: gzip` (or `br`) on the JS bundle; `/assets/*` has
  `Cache-Control: public, max-age=31536000, immutable`; `index.html` has `no-cache`.
- [ ] **OG image is live:** `https://queenscargo.com/brand/og-image.jpg` returns 200 (`image/jpeg`). Paste the home
  link in WhatsApp, then run it through the Facebook Sharing Debugger and LinkedIn Post Inspector ("Scrape again").
- [ ] View source of the live HTML and JS bundle: only Queens Cargo names, domain and email appear.
- [ ] Lighthouse mobile on Home meets the MOTION_3D_SPEC §2 targets.

## 8. After launch

- **Backups:** copy `queenscargo.db` **and the `UPLOADS_DIR` folder** (both in `queenscargo-data/`) daily to off-server
  storage (a Hostinger cron job + download, or a VPS cron to cloud storage). The database stores only file names; the
  photos and proofs live in the folder, so one without the other is not a full backup. Copy the database while the app is
  idle, or, where the `sqlite3` tool is installed, use `sqlite3 queenscargo.db ".backup queenscargo-backup.db"`. Test a restore once.
- **Updates:** push to `main` → Hostinger redeploys. Use a `staging` branch for bigger changes.
- **Monitoring:** a free uptime monitor on `https://queenscargo.com/api/health` (5-minute interval) with email
  alerts to info@queenscargo.com.
- **Security:** rotate `ADMIN_PASSWORD_HASH` and `SESSION_SECRET` if anyone leaves; keep dependencies updated
  (`npm audit` monthly). Remove `/api/diag/storage` once persistence is confirmed.
