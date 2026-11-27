# SHAYEGAN — Deployment runbook

Status at 2026-11-27: local prototype complete, Drive `Shayegan/` tree created,
GitHub account `aydaneighbor-commits` created and the GitHub connector connected.
Cloudflare account creation failed in Muse's cloud browser (human-verification
bot check + expired login card) — no Cloudflare account exists yet.

You do **not** need Cloudflare to go live. Two paths:

## Path A — GitHub Pages now (recommended first launch)

Free, always-on static hosting. No nameserver change needed; you only add DNS
records in the panel you already have (current nameservers:
NS1/NS2.HOSTING.BUSINESSIDENTITY.LLC). HTTPS is automatic via Let's Encrypt.

1. Confirm GitHub repo access for Muse (the connector card from 2026-11-27:
   choose "all repositories" since none exist yet), or create the repo
   yourself: name suggestion `shayegan-site`, public, no template files.
2. Push this folder's contents to the repo's `main` branch (root = site root).
   The `CNAME` file (kirtooshayegan.com) and `.nojekyll` are already included.
3. Repo → Settings → Pages → Source: "Deploy from a branch", branch `main`,
   folder `/ (root)` → Save. Set Custom domain: `kirtooshayegan.com` → Save.
4. In your current DNS panel (registrar/host), remove any default `@` A/CNAME
   records, then add (values from GitHub Docs, checked 2026-11-27):
   - `A  @  185.199.108.153`
   - `A  @  185.199.109.153`
   - `A  @  185.199.110.153`
   - `A  @  185.199.111.153`
   - `AAAA  @  2606:50c0:8000::153` (and `8001`, `8002`, `8003` variants)
   - `CNAME  www  aydaneighbor-commits.github.io`
   Do not use a wildcard `*.kirtooshayegan.com` record.
5. Wait for DNS propagation (up to 24h, usually faster), then back in
   Settings → Pages tick "Enforce HTTPS" once the certificate is issued.
6. Verify: `dig kirtooshayegan.com +noall +answer -t A` returns the four IPs
   above; open https://kirtooshayegan.com and https://www.kirtooshayegan.com.

Limits of Path A: static only. Accounts/tiers/payments and the Drive→catalog
sync still need a backend (see below). News updates = commit a new
`data/news.json` (Muse can do this on a schedule once approved).

## Path B — Cloudflare Pages later (optional upgrade)

Adds CDN, Workers/D1 for backend auth, and cron triggers in one place.

1. Create the Cloudflare account **on your own phone/computer** (the cloud
   browser is flagged as a bot by Cloudflare's verification; your own device
   will pass). Use the same Gmail via "Continue with Google".
2. Cloudflare dashboard → Add a site → `kirtooshayegan.com` → copy the two
   Cloudflare nameservers it assigns.
3. At your registrar, replace NS1/NS2.HOSTING.BUSINESSIDENTITY.LLC with the
   Cloudflare pair. (This moves DNS hosting to Cloudflare; the domain stays
   registered where it is. Keep any email MX records noted before switching.)
4. Workers & Pages → Create → Pages → Connect to Git → pick the same
   `shayegan-site` repo → framework preset: None → build command: (empty) →
   output directory: `/` → Deploy. Then Custom domains → add
   `kirtooshayegan.com` and `www.kirtooshayegan.com`.
5. Remove the GitHub Pages custom domain afterwards so the two hosts do not
   fight over the domain.

## Backend (still to build, either path)

- Auth: Google sign-in (OAuth) + unique usernames, Bronze/Gold/Diamond tiers,
  admin role with strong password + 2FA set via the provider's secure flow.
- Store: self-registration only; payment activates the tier on the user's own
  account; per-user `license.txt` generated after verified payment.
- Catalog sync: scheduled job reads Drive `Shayegan/<Section>/<Title>/info.json`,
  validates, regenerates `data/catalog.json`, commits/deploys.
- News job: daily curation writing `data/news.json` (10 stories/day), then deploy.
  Needs Parham's explicit approval for the recurring schedule + cadence.
- Secrets only in provider secret storage / Secure Vault — never in Drive,
  chat, or code. Rotate any password ever pasted into a chat.
