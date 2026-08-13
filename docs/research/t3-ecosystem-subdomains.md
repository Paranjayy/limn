# T3 Ecosystem Subdomain Research

Curated reconnaissance of the **T3 / pingdotgg (Theo)** agent tooling stack:

- **10xn.dev** — creator-network / media brand (T3 Agency)
- **postplan.dev** — authenticated static HTML draft publishing for agents
- **lakebed.dev / lakebed.app** — agent-native CLI + runtime for building and deploying full-stack TypeScript apps ("capsules") on `*.lakebed.app`

Research date: 2026-08-13. Sources: crt.sh (down), certspotter CT, Wayback CDX, web search, live HTTP probing + DNS wildcard checks.

---

## 0. Wildcard DNS & Routing Infrastructure (Round 2 Discovery)

A key finding from comprehensive DNS probing is that **all 4 domains use wildcard routing**:

- `*.lakebed.app` points to `bsxsznmc.up.railway.app` (Railway IP `69.46.46.101`).
- `*.postplan.dev` points to `qgxa1awy.up.railway.app` (Railway IP `69.46.46.71`).
- `*.lakebed.dev` points to a Fly-like network IP pool (`216.150.1.x` and `216.150.16.x`).
- `*.10xn.dev` points to the same `216.150.1.x`/`216.150.16.x` IP pool.

**Insight**: Because of wildcard DNS, *any* subdomain query resolves (e.g. `anything.lakebed.app` resolves to Railway). Brute-forcing subdomains confirms this catch-all behavior. The real app/capsule isolation happens at the HTTP routing layer (the ingress router reads the Host header and maps it to the target container/worker).

However, specific critical service subdomains are pinned to distinct Railway endpoints:
- `api.lakebed.dev` → `dmzcxtgj.up.railway.app`
- `auth.lakebed.dev` → `klb9hb90.up.railway.app`
- `dashboard.lakebed.dev` → `mij76mso.up.railway.app`

---

## 1. 10xn.dev — "The 10x network of channels for 10x devs"

T3's media/creator-network landing (brands pay to reach trusted builders). Subdomain footprint is small.

| Subdomain | Status | Title | Notes |
|---|---|---|---|
| `10xn.dev` | 200 | 10xn | Creator network landing page; roster includes Theo, Kent C. Dodds, NeetCode, Simon Willison, ForrestKnight, Acerola, etc. Contact/inquiry form. |
| `www.10xn.dev` | 200 | 10xn | Mirrors apex. |
| `soon.10xn.dev` | 404 | — | Previously a "coming soon" teaser page (archived/search-indexed). |
| `miles-pages.10xn.dev` | 404 | — | Pages deploy by a team member named Miles (fun insight: staff dogfood their own subdomain scheme). |
| `docs.10xn.dev` | 404 | — | Reserved/placeholder. |

---

## 2. postplan.dev — "Authenticated static HTML draft publishing for agents"

The first-gen agent publishing tool. An agent (Claude Code, etc.) uploads a single `plan.html` draft via CLI and gets a public URL. CLI: `npx postplan upload ./plan.html`.

### Core / service subdomains

| Subdomain | Status | Title | Notes |
|---|---|---|---|
| `postplan.dev` | 200 | Postplan | Home. Shows current draft count + health endpoint link. |
| `postplan.dev/healthz` | 200 | — | Health check. |
| `postplan.dev/dashboard` | 200 | Sign in | "My drafts" UI; sign-in via shoo; publishing stays anonymous unless a key is attached. |
| `postplan.dev/cli/auth` | 200 | — | CLI auth/onboarding flow. |
| `api.postplan.dev` | 200 | Postplan | Points to `qgxa1awy.up.railway.app` → backend hosted on **Railway**. |
| `app.postplan.dev` | 200 | Postplan | Railway-backed app host. |
| `www.postplan.dev` | 200 | Postplan | Railway-backed alias. |

### Deployed drafts (`*.postplan.dev` — random slug per deployment)

Dynamic subdomains; only a handful are archived/indexed (wildcard cert `*.postplan.dev`, so they don't show in CT logs).

| Deployment URL | Status | Title | Notes |
|---|---|---|---|
| `https://6xhul3gwy81u.postplan.dev/` | 200 | Filesystem platform comparison | Warm-store benchmarks for pnpm and XFS over VDO vs APFS macOS. |
| `https://tltqzg56k26j.postplan.dev/` | 200 | pstack fit audit | Fit-audit report for Cursor's **pstack** plugin skills. |
| `https://pmldo09w8l3r.postplan.dev/` | 200 | mattpocock skills fit audit | Fit-audit of **Matt Pocock**'s skills. |
| `https://hsyscdqldmk5.postplan.dev/` | 200 | T3 Code — Sidebar v2 Concepts | Theo's T3 Code product-design concept page. |

---

## 3. lakebed.dev — "Agent-native CLI and runtime for building and deploying Lakebed capsules"

The successor platform. A **capsule** = one complete app: `server/index.ts` + `client/index.tsx` (Preact) + `shared/` + optional `.env.lakebed.server`. Ships DB, auth (Google), object storage, logs, deploy. Source: `github.com/pingdotgg/lakebed`, npm `lakebed` (v0.0.29).

CLI: `npx lakebed new` / `dev` / `build` / `deploy` / `claim` / `inspect` / `db list|dump|export` / `logs` / `domains add`.

### Core / service subdomains (lakebed.dev)

| Subdomain | Status | Title | Notes |
|---|---|---|---|
| `lakebed.dev` | 200 | Lakebed | Marketing home. Also hosts `lakebed.dev/acceptable-use` and `lakebed.dev/abuse` (report form). |
| `www.lakebed.dev` | 200 | Lakebed | Alias of apex. |
| `docs.lakebed.dev` | 200 | Lakebed Docs | Full docs site + `llms.txt`, `llms-full.txt`, `docs.json`, raw markdown for agents. Docs sections: capsule-api, auth, storage, reference, examples (todo, guestbook). |
| `api.lakebed.dev` | 200 | Lakebed Deployments | Deploy API (the thing the CLI hits). |
| `auth.lakebed.dev` | 200 | Lakebed Auth — Identity for Lakebed Apps | First-party auth service (Google sign-in, stable user IDs across deploy hostnames). |
| `dashboard.lakebed.dev` | 200 | (SPA) | Dashboard — currently serving "Lakebed anonymous deploy runner" placeholder. |
| `admin.lakebed.dev` | 404 | — | Reserved admin subdomain. |
| `new.lakebed.dev` | 404 | — | Reserved (likely `npx lakebed new` docs/template origin). |

### Staging service subdomains

| Subdomain | Status | Title | Notes |
|---|---|---|---|
| `api.staging.lakebed.dev` | 200 | Lakebed Deployments | Staging deploy API. |
| `auth.staging.lakebed.dev` | 200 | Lakebed Auth — Identity for Lakebed Apps | Staging auth. |
| `dashboard.staging.lakebed.dev` | 200 | (SPA) | Staging dashboard. |
| `docs.staging.lakebed.dev` | 000 | — | Staging docs (down at scan time). |

---

## 4. lakebed.app — the deployed-capsule domain

Where every hosted capsule gets a subdomain. Users claim deploys and can reserve names via `npx lakebed domains add my-app.lakebed.app` (reserved: `api`, `admin`, `docs`, `www`). Auto-generated anonymous deploys get names like `adjective-noun-hex` (e.g. `bright-garden-ca744f`).

### Infrastructure subdomains

| Subdomain | Status | Title | Notes |
|---|---|---|---|
| `lakebed.app` | 404 | — | Apex (bare root returns 404; www serves the app). |
| `www.lakebed.app` | 200 | (SPA) | Served from `bsxsznmc.up.railway.app` (**Railway**). |
| `api.lakebed.app` | 200 | (anonymous deploy runner) | Deploy API endpoint; the CLI's `--api` origin. |
| `docs.lakebed.app` | 200 | (anonymous deploy runner) | Docs host. |
| `staging.lakebed.app` | 000 | — | Staging apex (down at scan time). |
| `lakebed.lakebed.app` | 200 | notarickroll | Dogfood capsule — "not a rickroll". |
| `llm-pps.lakebed.app` | 200 | deepswe-performance | DeepSWE worker prompt-per-second performance metrics tracking dashboard. |

### Community / demo capsules (named)

| URL | Status | Title | Notes |
|---|---|---|---|
| `https://theoideadump.lakebed.app/` | 200 | theoideadump | Theo's public idea-dump board. |
| `https://badlogic-list.lakebed.app/` | 200 | badlogic-list | List feed with **RSS** — "bad logic" takes / dev-logic rants. |
| `https://baker-wars.lakebed.app/` | 200 | baker-wars | Fun little app (baking battle?) — demo capsule. |
| `https://dinorip.lakebed.app/` | 200 | dinorip | Joke capsule (dino RIP). |
| `https://realcard.lakebed.app/` | 200 | realcard | Card-collector demo (real-cards, i.e. Pokémon-like). |
| `https://the-bee-movie.lakebed.app/` | 200 | bee-script | Bee Movie script app (meme). |
| `https://lakebed.lakebed.app/` | 200 | notarickroll | Rickroll meme capsule. |
| `https://quiet-river-99404a.lakebed.app/` | 200 | pigeon-parliament | Auto-named anonymous deploy (pigeon parliament). |
| `https://silver-river-a9ce076279.lakebed.app/` | 410 | akron-classifier | Expired/unclaimed deploy (HTTP 410 Gone). |
| `https://rickrolls-per-stream.lakebed.app/` | 200 | Lakebed Capsule | Meme-metric capsule (rickrolls per stream). |
| `https://rapid-signal-65ad0f.lakebed.app/` | 200 | Browser Runtime Sandbox | Auto-named anonymous deploy. |

### Auto-named anonymous deploys ("Lakebed App Factory Hub")

Many archived/live auto-named capsules all render a shared template titled **"Lakebed App Factory Hub"** — i.e. the default anonymous-deploy landing. Current scans:

| URL | Status |
|---|---|
| `open-meadow-efee79.lakebed.app` | 200 |
| `clear-signal-2e7d5a.lakebed.app` | 200 |
| `frosted-garden-88b4ab.lakebed.app` | 200 |
| `quiet-meadow-19ce56.lakebed.app` | 200 |
| `bright-harbor-52b34a.lakebed.app` | 200 (archived) |
| `frosted-orbit-d08f40.lakebed.app` | archived |
| `rapid-ridge-05a241.lakebed.app` | archived |
| `silver-signal-4c2d4c.lakebed.app` | archived |

### Expired / dead auto-named deploys (404 or unreachable)

`bright-garden-ca744f`, `bright-harbor-52b34a`, `clear-harbor-f633b0`, `clear-orbit-c6a7f6`, `frosted-orbit-d08f40`, `frosted-river-9810d4`, `open-garden-47fc60`, `rapid-ridge-05a241`, `silver-signal-4c2d4c`, `silver-river-a9ce076279` (410), `realcard` (timeout). Consistent with the documented lifecycle: unclaimed deploys expire, stop serving, then get deleted ~1 week later.

---

## 5. Adjacent / lookalike domains (NOT T3 — do not confuse)

| Domain | What it is | Why it's relevant |
|---|---|---|
| `lakebed.io` | "Data lakes made easy" — a completely different data-analytics product. | Name-collision lookout. |
| `postplan.app` / `postplan.io` | Instagram/social media schedulers. | Name-collision lookout. |
| `lakebed-native.xyz` | **Lakebed Native** — a community/secondary project: React Native apps that deploy onto the Lakebed hosted backend ("0 native projects per capsule", OTA JS updates). | Signals ecosystem expansion beyond web capsules. |
| `github.com/pingdotgg/lakebed` | Lakebed source repo. | Reference for building our own. |
| `web-harmonium.com` / `rajaramaniyer.github.io` | Web Harmonium (Rajaraman Iyer) — unrelated, separate project we're also building on. | Adjacent build target. |

---

## 6. Key takeaways for our own build (a "lakebed/postplan/hermes-like" thing)

1. **Naming scheme is the product surface**: `*.postplan.dev` = anonymous slug deploys; `*.lakebed.app` = named/claimed capsules + auto-named deploys. We can ship a similar scheme without the name (our own domain).
2. **Wildcard cert + dynamic subdomains**: `*.postplan.dev`, `*.lakebed.app`, `*.lakebed.dev` all wildcard — CT logs are useless for enumerating live deploys; Wayback CDX + search engines are the real source.
3. **Backend stack tells**: postplan + lakebed.app use **Railway** (`*.up.railway.app`); lakebed.dev + 10xn.dev use a Fly-ish `216.150.16.x` network.
4. **Agent-first docs**: docs ship `llms.txt`, `llms-full.txt`, `docs.json`, raw markdown endpoints — the docs are written for agents, not just humans. Must-have for an agent-native platform.
5. **Capsule contract is minimal**: one server file, one client file, shared pure-TS, env file. Small surface = agents can build+deploy without leaving code.
6. **Expiry model**: unclaimed deploys expire (410 → delete). Cheap, abuse-resistant, scalable.
7. **Dogfooding**: Theo deploys his own capsules (`theoideadump`, `lakebed.lakebed.app`) — the platform is also its own first customer.