# Wasatch Mountain Riders — Coach Resources

Workspace for the WMR youth XC mountain biking coach prep site. Self-contained static HTML — no build step, no server, opens directly in a browser.

## Layout

```
wmr-coaching.com/
├── CLAUDE.md                               # this file (workspace-only, not deployed)
├── vercel.json                             # Vercel deploy config
├── .vercelignore                           # excludes .md and metadata from deploy
└── initial_webpage_resources/              # ← served as site root by Vercel
    ├── index.html                          # landing → 7 decks + reference
    ├── reference.html                      # Protocols / Stuck / Readiness / Standards
    ├── lil_shredders_skill_cards.html      # ages 5–7, coed
    ├── beginner_boys_skill_cards.html      # ages 7–9
    ├── beginner_girls_skill_cards.html     # ages 7–9
    ├── intermediate_boys_skill_cards.html  # ages 8–11
    ├── intermediate_girls_skill_cards.html # ages 8–11
    ├── advanced_boys_skill_cards.html      # ages 9–12
    ├── advanced_girls_skill_cards.html     # ages 9–12
    ├── robots.txt                          # allows indexing, points at sitemap
    ├── sitemap.xml                         # 9 public URLs
    ├── wmr_what_we_learned.md              # curriculum rationale — NOT deployed
    └── README.md                           # coach-internal usage notes — NOT deployed
```

Each deck holds 12 skill cards for a 10-week program. Mobile-optimized and print-friendly (each card prints on its own page).

## Design system

**Palette** — warm paper-and-ink:
- `--paper #F5F1E8`, `--paper-light #FAF7F0`, `--paper-elev #FBF8F1`
- `--ink #1C1816`, `--ink-soft #3A332D`, `--ink-secondary #5C544A`, `--ink-faded #8B8275`
- `--rule #C9C0AC`, `--rule-soft #DDD5C2`
- `--sienna #B8543F` (primary accent), `--sienna-deep #8B3E2D`, `--sienna-pale #F0DDD2`, `--sienna-bg #F8EBE2`
- `--stone #EDE6D5`

**Type**:
- Display: `Fraunces` (variable: opsz 9–144, wght 300–900, SOFT 0–100)
- Body: `Source Serif 4` (variable: opsz 8–60, wght 300–700)
- Italics + sienna for emphasis in headings (`<em>` inside `<h1>`/`<h2>`/`<h3>`)

**Layout tokens**:
- `--content-max 1080px` (index), `760px` (deck pages)
- `--reading-max 640px` (single-column reading width)
- Section padding: 5rem desktop / 3rem mobile (≤640px breakpoint)

## Working conventions

- Edit existing files; don't introduce new ones unless asked.
- Keep self-contained — only external dependency is Google Fonts.
- Design tokens are duplicated across files. When changing palette/type, update every file (7 decks + index + reference = 9 places).
- Print-first thinking: each card should print cleanly on its own page.
- Read this file and any rationale doc at session start before content edits — many content choices are deliberate and look arbitrary in isolation.

## Content guardrails

Source of truth for the *why* behind each rule: [initial_webpage_resources/wmr_what_we_learned.md](initial_webpage_resources/wmr_what_we_learned.md). Read it before content edits.

- **No feature dimensions in writing.** Drop heights, jump sizes, gap distances — never a number. Always "coach's judgment." (Liability.)
- **"Front Wheel Lift" ≠ "Manual."** Beginner cards teach Front Wheel Lift; Advanced cards teach Manual. Don't conflate. (NICA-aligned.)
- **"Falling Safely" appears only in Lil' Shredders Card 01.** Developmentally defensible at 5–7; not in older decks, not in formal NICA curriculum.
- **Flat pedals through Advanced.** No clipless in any deck. (Clipless creates bad bunny hop patterns; needs a separate parent conversation.)
- **No doubles, gap jumps, wood drops, or step-ups in any deck.** Saved for high school NICA — out of scope here.
- **Boys & girls (7+) share identical skills and identical drill names.** Differ only in coaching language (challenge framing vs capability framing). Don't introduce skill differences between gendered decks.
- **Drills marked with † appear in multiple decks on purpose** — same name, harder with age. Don't rename to disambiguate.
- **Drill names describe what the kid is doing**, not what skill is taught ("Rolling Attack," not "Attack Position Drill"). Verb matches action.
- **Coach demos are conditional.** "If you're confident, demo it. If not, walk it with them." Never frame demos as required.
- **No body comments from coaches — even well-intentioned ones.** Specific named driver of the girls' retention cliff at 11–14.
- **Pro role-model names go stale ~5 years out.** Current names (Blevins, Amos, Pidcock; Batten, Pieterse, Ferrand-Prévot, Richards) will need refresh planning. Flag if updating.

## Open threads

_Things mentioned but not yet acted on. Trim when resolved._

- _(none)_

## Deployment

Static site, deployed to **Vercel** at the apex domain **wmr-coaching.com** (user owns the domain).

- **GitHub repo:** [wasatchmountainriders/wmr-coaching.com](https://github.com/wasatchmountainriders/wmr-coaching.com) (public)
- **Vercel project:** `wmr/wmr-coaching.com` ([dashboard](https://vercel.com/wmr/wmr-coaching.com))
- **Vercel-issued URL** (works immediately): https://wmr-coachingcom.vercel.app
- **Custom domain** (live once DNS propagates): https://wmr-coaching.com
- Git-connected — every push to `main` auto-deploys to production; every PR gets a preview URL.

### How it's wired

- `vercel.json` (workspace root) — Vercel config. Points `outputDirectory` at `initial_webpage_resources/` so that folder is served as the site root. No build step.
- `.vercelignore` (workspace root) — excludes `CLAUDE.md`, all `**/*.md` (rationale doc + READMEs), and macOS metadata from the upload.
- `initial_webpage_resources/robots.txt` — allows all crawlers, references the sitemap.
- `initial_webpage_resources/sitemap.xml` — 9 canonical URLs at `https://wmr-coaching.com/...`.

### URL structure

`cleanUrls: true` strips `.html` from public URLs. So:

- `index.html` → `https://wmr-coaching.com/`
- `lil_shredders_skill_cards.html` → `https://wmr-coaching.com/lil_shredders_skill_cards`
- `reference.html` → `https://wmr-coaching.com/reference`

Internal `<a href="*.html">` links in the HTML still resolve, via a 308 redirect to the clean form. One redirect per nav click — acceptable tradeoff to keep HTML untouched.

### Headers / security

Set in `vercel.json` and apply to every response:

- **HSTS** — `Strict-Transport-Security: max-age=31536000; includeSubDomains` (1 year, no preload — preload is hard to undo)
- **X-Content-Type-Options: nosniff**
- **X-Frame-Options: SAMEORIGIN**
- **Referrer-Policy: strict-origin-when-cross-origin**
- **Permissions-Policy** — camera, microphone, geolocation, interest-cohort all disabled
- **CSP** — permissive enough to allow the existing inline `<style>` / `<script>` blocks and Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`). Blocks `<object>` / `<embed>`, restricts `base-uri` and `form-action` to self.
- HTML responses set `Cache-Control: public, max-age=0, must-revalidate` — content updates are visible immediately.
- `robots.txt` / `sitemap.xml` cache 1 hour.

### Deploying

Two paths:

1. **Git-connected (preferred for production)** — push this directory to a GitHub/GitLab/Bitbucket repo, import the repo in Vercel dashboard. Every push to `main` deploys; every PR gets a preview URL. Audit trail + rollback.
2. **CLI (for one-off pushes)** — `npm i -g vercel`, then `vercel` from the workspace root for a preview deploy, `vercel --prod` to push to production.

### Custom domain

Owned: **wmr-coaching.com**. In the Vercel dashboard:

1. Project Settings → Domains → add `wmr-coaching.com` and `www.wmr-coaching.com`.
2. Vercel will display the DNS records needed. Typically:
   - **Apex** (`wmr-coaching.com`): `A` record → Vercel's IP (Vercel shows the current value)
   - **www** (`www.wmr-coaching.com`): `CNAME` → `cname.vercel-dns.com`
3. Set those records at the domain registrar.
4. Pick apex as the **primary** in Vercel — `www` gets auto-redirected to the apex by Vercel.

HTTPS is automatic (Let's Encrypt via Vercel).

## Changelog

Append meaningful changes here. Newest at top. Format: `YYYY-MM-DD — short description`.

- **2026-05-14** — First production deploy live at https://wmr-coachingcom.vercel.app. Created public GitHub repo [wasatchmountainriders/wmr-coaching.com](https://github.com/wasatchmountainriders/wmr-coaching.com), linked to Vercel project `wmr/wmr-coaching.com`, attached both `wmr-coaching.com` and `www.wmr-coaching.com` to the project. Custom domain awaits DNS at Squarespace (A record `@` and `www` → `76.76.21.21`).
- **2026-05-14** — Wired up Vercel deploy for the public custom domain `wmr-coaching.com`. Added `vercel.json` (cleanUrls, security headers, CSP allowing inline styles/scripts + Google Fonts), `.vercelignore` (excludes `.md` files from deploy), `robots.txt`, and `sitemap.xml`. No HTML files touched.
- **2026-05-14** — Rationale doc restored at [initial_webpage_resources/wmr_what_we_learned.md](initial_webpage_resources/wmr_what_we_learned.md). Backfilled content guardrails from it.
- **2026-05-14** — Created `CLAUDE.md` for workspace-persistent context. Initial assets in [initial_webpage_resources/](initial_webpage_resources/): 7 program decks, universal reference, landing page, README.
