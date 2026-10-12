# Queens Cargo: Prompt Pack for Claude in VS Code

**The job:** launch this working logistics platform as a **brand-new site, Queens Cargo**, on **queenscargo.com**
(Hostinger Node.js app). Only these things change: **name, logo, domain, email and tracking/reference prefix**.
**These stay exactly the same:** colour palette, fonts, layout, every photo, every page, every feature, the
tagline and the admin console path (`/private-user/`). Target time: **about 1 hour**.

**How to use**
- Send the prompts **in order, one per chat turn**. Each one ends with STOP. Check the result (`npm run dev` →
  http://localhost:3000) before you send the next one.
- If something breaks, send **Prompt F (Fix)**. In a new chat, send **Prompt R (Resume)** first.
- Time budget: P1 5′ · P2 10′ · P3 12′ · P4 8′ · P5 7′ · P6 3′ · P7 15′ ≈ **60 min**.

> ⛔ **Fresh site, fresh identity.** Queens Cargo gets a **new GitHub repo**, a **new Hostinger Node.js app**, a
> **new empty database** and **new admin credentials**. The previous brand's name, domain, email, repo and
> app are **never** mentioned in code, docs, metadata, git history or commit messages, and nothing is pushed to
> the previous repo or app.

---

## Project values (source of truth: confirmed by the owner on 2026-10-12)

| Key | Value |
|---|---|
| Name / legal name | **Queens Cargo** (written without an apostrophe in all text; the logo artwork reads "QUEEN'S" and stays as supplied) |
| Short name (UI) | **Queens Cargo** (confirmed by the owner: the full name is also the short name) |
| Tagline | **Faster Moves. Bigger Reach.** (unchanged; printed on the logo) |
| Domain | **queenscargo.com** (`www.` redirects to the root domain) |
| Email | **info@queenscargo.com** |
| Admin console | **https://queenscargo.com/private-user/** (same app, same domain, path-based; already built) · locally `http://localhost:3000/private-user/` **and** `http://localhost:3000/#/admin` |
| Admin console name | **Queens Cargo Operations Console** (from `COMPANY_SHORT`) |
| Tracking ID | **`QS` + 6 characters = 8 total**, e.g. `QS7K2M9P`; piece labels `QS7K2M9P-01` |
| Reference prefixes | `QS-TKT-######`, `QS-INV-######`, `QS-SL-######` (confirmed by the owner; quotes keep `QR-YYYY-#####`) |
| Logo source | `images/queenscargo.png`: 2030×775, **RGB, NOT transparent**. The light-grey/white checkerboard (≈ RGB 230–245) is baked into the pixels and must be removed. Red crown + globe, black "QUEEN'S", red "CARGO", small black tagline, thin white outlines around the letters. |
| Palette, fonts, photos | **Unchanged.** Red `#D3070B` + ink `#181818` + greys already match the new logo. No photo changes. |
| DB file (default) | `queenscargo.db` (production uses `DB_PATH`). Fresh, empty database. Table/column names unchanged. |
| Internal code names **kept** | CSS `--ngf-*`, classes `ngf-*`, storage keys `ngf_*`, session cookie `ngf.sid` (in `server/middleware/auth.ts`). Users never see them. Optional after launch: Prompt 9. |
| Phone / WhatsApp / HQ address / social | **Not supplied. Leave empty in `brand.ts`, so the UI keeps hiding them.** Never invent them. Add later with Prompt O. |
| Old brand terms | Whatever `src/config/brand.ts` holds **before** Prompt 3 (name, short name, domain, email, tracking/reference prefix, DB file name). Prompt 1 reads them and keeps the list **in the chat only**. Never write it into a committed file. |

---

### Prompt 1: Safety net, branch and remotes (no code changes, ~5 min)
```
We're launching this working site as a brand-new site, "Queens Cargo", on queenscargo.com. Read PROMPTS.md (its values table is the source of truth), CLAUDE.md, docs/PROJECT_TRACKER.md, src/config/brand.ts, src/shared/trackingId.ts, src/shared/references.ts, scripts/optimize-images.mjs, server/seo.ts, server/index.ts and docs/DEPLOYMENT.md.

Rules for the whole job: don't change the design, colours, fonts, layout, photos, tagline or features. Only the name, logo, domain, email and tracking/reference prefix change. The previous brand's name, domain, email and repo must not appear anywhere in the finished site, docs, commit messages or git history. Never push to the previous repo.

1. Create branch `queenscargo` from the current HEAD. If PROMPTS.md, docs/PROJECT_TRACKER.md and the images/ logo swap (old logo file deleted, images/queenscargo.png added) are the only uncommitted changes, commit them as "docs: Queens Cargo launch plan and new logo source". Any other uncommitted change: STOP and ask me.
2. Run `git remote -v`, then REMOVE every remote from this local clone so nothing can be pushed to the previous repo by mistake. This changes only local git config. Don't write the remote URLs into any file.
3. `npm run build` and `npm test`: record the baseline.
4. Read the current brand values from src/config/brand.ts (name, short name, domain, email, tracking prefix, reference prefix) and the DB file name in server/db.ts. These are the "old brand terms". Count them (case-insensitive) in src, server, scripts, index.html, Public, docs, CLAUDE.md, package.json, package-lock.json and .env.example, grouped as: visible text & metadata / documents & PDFs / tracking & references / docs. Skip the kept internal code names (ngf-, --ngf-, ngf_, ngf.sid).

STOP and report: baseline result, the old-term list and counts per group (in chat only), and any questions.
```

### Prompt 2: New logo and icons (~10 min)
```
Replace the logo with images/queenscargo.png. Keep the same placements and sizes; no layout change.

IMPORTANT: this source is NOT transparent. It is a 2030×775 RGB PNG with a light-grey/white checkerboard (about RGB 230–245, neutral) baked into the pixels, so it must be keyed out.

1. scripts/optimize-images.mjs: set LOGO_SRC to 'queenscargo.png' and update the header comment (file name, size, "not transparent"). Replace the haze-only loadTransparentLogo with a background removal that:
   - makes near-white, low-saturation pixels transparent (the checkerboard), with a soft ramp (e.g. fully transparent above ~225 brightness with max-min channel spread < ~18, partial alpha just below that) so edges stay smooth with no grey fringe or halo;
   - never touches red pixels (use the existing redness() / RED_MIN) or the black wordmark;
   - after keying, has no leftover checkerboard specks: flood-fill from the canvas edges so only background connected to the border is removed, and keep the thin white outlines around the letters where they are enclosed. If the outlines can't be kept cleanly, it's fine to drop them; tell me.
   Then trimTransparent() as today. Don't upscale.
2. Run `node scripts/optimize-images.mjs --brand` (brand only: the photo originals are not in this folder, so never run the full script). It must produce, in Public/brand/:
   - logo.png: full colour on transparent, for light backgrounds.
   - logo-white.png: black/dark parts ("QUEEN'S" and the tagline) turned white, red kept, for dark backgrounds (header over the hero, footer, admin sidebar and login).
   - mark.png: a square icon cropped from the red globe with the box and orbit arrow (crown optional if it reads better at 16 px).
   - favicon.png (512), favicon-32.png, favicon-16.png, icon-192.png, icon-maskable-512.png (safe padding), apple-touch-icon.png (180), all from mark.png. Update buildSquareMark's crop box for the new artwork and check the 16/32 px icons are recognisable.
   - og-image.jpg (1200×630): the white logo centred on Ink, as before.
3. Composite logo.png on pure white AND on #181818 and look at both at 2× zoom: no checkerboard, no grey halo, no missing pieces. Show me both.
4. Bump the ?v=N cache-bust in index.html and Public/site.webmanifest.
5. The new logo is about 2.6:1 and its tagline is small. Check the header (desktop and mobile), mobile drawer, footer, admin sidebar and login, one PDF and the browser tab at 375 and 1440 px. If it reads too small or too large somewhere, adjust ONLY that logo's height/width rule.

`npm run build` must pass. Commit "brand: Queens Cargo logo and icons". STOP and show me the logo on light and dark backgrounds and the favicon.
```

### Prompt 3: Name, domain, email and tracking/reference prefix (~12 min)
```
Apply the values table everywhere users, documents or crawlers can see it. Keep all copy word for word apart from the name, domain and email. The tagline is unchanged. No new marketing text.

1. src/config/brand.ts: COMPANY, LEGAL_NAME and COMPANY_SHORT "Queens Cargo", TAGLINE stays "Faster Moves. Bigger Reach.", EMAIL "info@queenscargo.com", DOMAIN "queenscargo.com", TRACKING_PREFIX "QS", REFERENCE_PREFIX "QS". ADMIN_PATH stays '/private-user/'. PHONE, WHATSAPP, HQ_ADDRESS and SOCIAL stay empty. Update the comments (e.g. "prefix + 6 characters, 8 total", "QS-TKT-123456").
2. Tracking IDs must stay EXACTLY 8 characters with a 2-letter prefix. In src/shared/trackingId.ts replace the fixed SUFFIX_LENGTH = 5 with ID_LENGTH = 8 and SUFFIX_LENGTH = ID_LENGTH - TRACKING_PREFIX.length (= 6 for QS), so the regex, generator, normaliser and piece-label parsing all follow. Update every comment and example (QS7K2M9P, QS7K2M9P-01, "qs 7k2-m9p" → "QS7K2M9P"), TRACKING_ID_EXAMPLE in brand.ts, input placeholders ("QS······"), maxLength/minLength attributes and any hard-coded "8"/"5"/"3" length checks in src and server (grep for them), help text, and scripts/trackingId.test.ts (8 chars total, prefix QS, 6 random chars, the 2-digit piece label). Fix the "32^5" capacity comment in server/trackingIds.ts (now 32^6). References: QS-TKT / QS-INV / QS-SL; update scripts/references.test.ts. Run `npm test`.
3. Hard-coded strings: index.html (title, description, canonical https://queenscargo.com/, OG/Twitter title "Queens Cargo: Faster Moves. Bigger Reach.", URLs and image, JSON-LD Organization name/url/email/logo/slogan), Public/site.webmanifest (name, short_name), App.tsx page titles/descriptions, page copy, image alt text, src/data/legalDocs.ts, DocumentBrand.tsx and every document/PDF label, server/index.ts (health service name, start log), server/seo.ts (sitemap/robots host), server/db.ts (settings defaults, default DB file data/queenscargo.db), .env.example (DB_PATH example), package.json name "queens-cargo" (regenerate package-lock.json with `npm install --package-lock-only`). In .ts/.tsx, import from brand.ts; don't hard-code the new strings.
4. docs/CONTENT.md and docs/BRAND_GUIDE.md: company facts, OG title, footer brand block, logo description (§6), ID formats (§7: QS + 6 = 8 characters; QS-TKT/INV/SL). CLAUDE.md: title, §1–§2 company-facts table (admin URL https://queenscargo.com/private-user/, tracking prefix `QS` + 6 characters, e.g. `QS7K2M9P`, references `QS-TKT-`, `QS-INV-`, `QS-SL-`), §6 rule 10 DB file name queenscargo.db, and every other mention of the old brand.
5. Grep again for every old brand term from Prompt 1 (case-insensitive) in src, server, scripts, Public, index.html, docs, CLAUDE.md, package.json, package-lock.json and .env.example. Expected: zero hits outside the kept ngf code names.

`npm run build` and `npm test` must pass. On localhost: Home, Track, Contact and Legal at 375 and 1440; create a shipment in the admin (#/admin) and confirm it gets an 8-character QS ID that tracks publicly (also as lower case with spaces, and as a -01 piece label); download one PDF and check the name/logo/references. Commit "brand: Queens Cargo name, domain, email and IDs". STOP and report.
```

### Prompt 4: Admin console and production-mode check on /private-user/ (~8 min)
```
The admin console already lives at /private-user/ on the main domain (ADMIN_PATH in brand.ts, isAdminPath() in App.tsx, the /private-user routes in server/index.ts, server/seo.ts). Don't rebuild it and don't edit server/middleware/auth.ts or the session settings. Only verify it works under the new brand and fix anything the rename broke.

1. Confirm the admin UI shows "Queens Cargo Operations Console", the new white logo in the sidebar and login, and ADMIN_URL https://queenscargo.com/private-user/ wherever the admin URL is printed (Settings/help).
2. Test with `npm run build && npm start` (production mode: NODE_ENV=production in the shell only, scratch DB_PATH and UPLOADS_DIR in the scratchpad, never the real data/ folder):
   - /private-user and /private-user/ open the login; /private-user-x and /private-users don't; wrong password is rejected; login works;
   - create a shipment → QS ID → "view public tracking" opens /#/track/<id> on the same host;
   - /#/admin and /admin show Home;
   - /private-user/ responses carry X-Robots-Tag noindex, nofollow;
   - robots.txt and sitemap.xml use https://queenscargo.com and don't mention private-user;
   - the public pages look unchanged apart from name and logo.
3. If any check fails, apply the smallest fix and commit "fix: …". STOP and report each check.
```

### Prompt 5: Docs, final sweep and full local check (~7 min)
```
1. docs/DEPLOYMENT.md: rewrite for Queens Cargo. One domain (queenscargo.com + www redirect), admin at https://queenscargo.com/private-user/, a NEW GitHub repo and a NEW Hostinger Node.js app, env vars (DB_PATH and UPLOADS_DIR in /home/<user>/queenscargo-data/), email info@queenscargo.com, the smoke test (§7) with the /private-user/ checks and QS IDs, backups of queenscargo.db + uploads.
2. docs/PROJECT_TRACKER.md: this is a fresh site. Rewrite it so it doesn't name the previous brand, domain, email, repo or ID prefix anywhere (history included): keep the phases and task wording with Queens Cargo values, replace the old change log with a single "2026-10-12 Queens Cargo launch" section listing the work done in Prompts 1–5, and keep Blocked / Needs owner current (phone, WhatsApp, HQ address, social links). docs/PROMPT-UPDATE.md and docs/update.md: if they're old prompt packs or notes for the previous brand, delete them; otherwise rewrite them for Queens Cargo. Tell me which.
3. Fresh `npm run build`, then sweep src, server, scripts, Public, index.html, docs, CLAUDE.md, PROMPTS.md, package.json, package-lock.json, .env.example AND dist/ + dist-server/ for the old brand terms from Prompt 1 (case-insensitive). Also check image metadata isn't an issue (Public/brand files are regenerated, so they're fine). Goal: ZERO hits anywhere except the kept internal ngf code names. List anything left and why.
4. Full run on localhost (production build, scratch DB): admin login at /private-user/ → create shipment (QS ID) → track publicly → change status → generate each document type → quote request → publish → public quote page → contact message → Messages inbox → package photo + payment request → proof upload → approve. Confirm the new name and logo everywhere and that nothing else changed visually (375/768/1440).
5. Confirm there's no demo data in code or seed, and SEED_DEMO_DATA is unset/false.
Commit "docs: Queens Cargo deployment and tracker" (and "fix: …" for any fixes). STOP with a short ready / not-ready report.
```

### Prompt 6: New admin password and session secret (~3 min, mostly me)
```
Set a new admin password and session secret for Queens Cargo. Configuration only; don't edit auth logic, and never ask me for the password.

1. Give me the two commands to run MYSELF in the project folder:
   - node -e "console.log(require('bcryptjs').hashSync('MY-NEW-PASSWORD', 12))"
   - node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
2. Tell me exactly what to put in my local .env (ADMIN_PASSWORD_HASH, SESSION_SECRET), whether the `$` characters in the hash need quoting, and remind me the same values go into Hostinger in Prompt 7. They must be NEW values, not ones used by any previous site.
3. After I say "done", restart and have me confirm: the old password is rejected, the new one works at http://localhost:3000/private-user/, and logout/login works.
STOP.
```

### Prompt 7: New GitHub repo and deploy to queenscargo.com on Hostinger (~15 min, mostly me in hPanel)
```
Prepare the deploy per docs/DEPLOYMENT.md. Don't deploy anything yourself; give me exact steps. This is a NEW site with a NEW repo, app and database.

1. GitHub, fresh history (no previous-brand commits or messages): check .gitignore excludes node_modules, dist, dist-server, data, .env, then give me the commands: `git checkout --orphan queenscargo-main`, `git add -A`, commit "Queens Cargo: initial release", `git branch -M main`, `git remote add origin <MY NEW EMPTY REPO URL>`, `git push -u origin main`. Ask me for the repo URL; don't guess it. Before pushing, confirm `git remote -v` shows ONLY the new repo and `git log --all --oneline` on the new main has exactly one commit. Tell me the old local branches can be deleted after the push (`git branch -D …`) so they're never pushed by accident.
2. hPanel → new Node.js web app: connect the new repo, branch main, Node 22.x (≥ 22.5), install `npm install` (postinstall builds; don't build twice), start `npm start` (entry dist-server/server/index.js).
3. Environment variables: NODE_ENV=production, PORT only if hPanel requires it, ADMIN_PASSWORD_HASH and SESSION_SECRET (from Prompt 6), DB_PATH=/home/<user>/queenscargo-data/queenscargo.db and UPLOADS_DIR=/home/<user>/queenscargo-data/uploads (a folder OUTSIDE the deploy directory; create it first), SEED_DEMO_DATA unset. The database starts empty.
4. Domain: queenscargo.com on this app, www.queenscargo.com redirecting to the root (one host, so the admin session cookie works), SSL on both, force HTTPS; DNS records if the domain is registered elsewhere.
5. Smoke test on the live domain (DEPLOYMENT §7): new logo, name and tab icon; the share preview uses queenscargo.com and the OG image loads; https://queenscargo.com/private-user/ shows the login and works with the new password; /#/admin and /admin show Home; robots.txt and sitemap.xml use queenscargo.com and don't mention private-user; log in, then open /api/diag/storage and confirm DB_PATH and UPLOADS_DIR point at queenscargo-data; create → track a QS shipment; redeploy once and confirm the shipment is still there; response headers show compression and the /assets cache rule.
6. Email: create the info@queenscargo.com mailbox and add SPF/DKIM/DMARC (Hostinger email or my provider). Send a test from the Contact form path if mail sending is configured.
STOP and wait for my results, then tick the launch tasks in the tracker and commit "docs: Queens Cargo live".
```

---

## Utility prompts

### Prompt R: Resume in a new chat
```
Resume the Queens Cargo launch. Read CLAUDE.md, PROMPTS.md and docs/PROJECT_TRACKER.md, run `git status`, `git branch` and `git log --oneline -10`, then tell me which prompt we're on and what's next. Don't change anything until I confirm.
```

### Prompt F: Fix something that broke
```
Something broke: [what you see, which page/URL, steps, any error message].
Find the root cause (check the latest commits first) and explain it before fixing. Apply the smallest fix that doesn't change design or unrelated behaviour. Confirm `npm run build` and `npm test` pass and that admin login at /private-user/ → create → track (QS ID) still works, then commit "fix: …".
```

### Prompt O: Owner contact details arrived
```
Here are Queens Cargo's contact details: [phone / WhatsApp / HQ address / social links].
Add them to src/config/brand.ts (and the JSON-LD in index.html), confirm the UI that was hidden now shows them correctly on the Header, Footer, Contact and documents, update the values table in PROMPTS.md and the tracker, build, and commit "content: add Queens Cargo contact details".
```

### Prompt 9 (optional, after launch): Rename the internal ngf code names
```
Rename the internal code names: CSS --ngf-* → --qc-*, class ngf- → qc-, storage keys ngf_* → qc_*, the cookie ngf.sid → qc.sid (SESSION_COOKIE and the logout clearCookie must change together; this logs everyone out once), and the Cookie Policy text. Update CLAUDE.md §7. It's a mechanical rename with no behaviour change: prove it by comparing the built bundles with the prefixes mapped back. Build, test, check 375/1440, commit "chore: rename internal ngf names to qc".
```
