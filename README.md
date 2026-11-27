# SHAYEGAN — kirtooshayegan.com

English cyberpunk/neon site prototype. All artwork is inline SVG.
Catalog entries are metadata only; no media files are served.

## Layout

- `index.html`, `styles.css`, `app.js` — static frontend (GitHub Pages / Cloudflare Pages-ready)
- `CNAME`, `.nojekyll` — GitHub Pages custom-domain files (kirtooshayegan.com)
- `DEPLOY.md` — deployment runbook (Path A GitHub Pages, Path B Cloudflare)
- `data/catalog.json` — generated catalog (movies / series)
- `data/news.json` — daily AI & gaming news feed (10 curated stories/day)
- `drive-structure/` — Google Drive folder schema + `info.json` template

## How the auto-catalog works (production)

1. Library lives in Drive under `Shayegan/<Section>/<Title (Year)>/info.json`.
2. A sync job (Cloudflare Worker cron or Muse scheduled job) reads the
   tree via the Drive API, validates each `info.json`, and regenerates
   `data/catalog.json`.
3. The static site renders whatever is in `catalog.json`. Adding a folder
   in Drive is enough — no site code changes.

## Deployment checklist

- [x] Create the `Shayegan/` folder tree in Drive from `drive-structure/`
      (done 2026-11-27: Movies, Series, Games, Apps, Projects, News).
- [x] Create GitHub account (done 2026-11-27: `aydaneighbor-commits`,
      https://github.com/aydaneighbor-commits; connector connected).
- [x] First curated news drop in `data/news.json` (done 2026-11-27, 10 stories).
- [ ] Confirm which repositories Muse may access (connector card pending),
      create the `shayegan-site` repo, push this folder, enable GitHub Pages.
- [ ] Point DNS at the host — see `DEPLOY.md` Path A (GitHub Pages, no
      nameserver change) or Path B (Cloudflare Pages, nameserver change at
      the registrar). Current NS: NS1/NS2.HOSTING.BUSINESSIDENTITY.LLC.
- [ ] Backend for accounts: Google sign-in (OAuth), unique usernames,
      Bronze/Gold/Diamond tiers, admin role. Secrets only in provider
      secret storage / Secure Vault — never in Drive, chat, or code.
- [ ] News job: daily curation writing `data/news.json`, then deploy
      (recurring schedule needs Parham's approval first).

## Security notes

- No passwords in this repo, in Drive, or in chat. Rotate any password
  that was ever pasted into a chat.
- Admin access is a backend role with a strong password + 2FA, set
  through the provider's secure flow.
- Account resale/giveaway is out of scope. The "Free & Official" section
  only points visitors to each service's own official signup.
- "Shayegan Store" sells only Shayegan's own subscriptions (site + future
  Shayegan app): users self-register with their own email/password and
  payment activates the tier on their own account. No pre-made Gmail
  accounts or passwords are sold or handed out, under any name.
- Paid text download (`license.txt`, sample in `downloads/`): after a
  verified payment the backend generates a per-user text file holding
  that user's own Shayegan activation key and instructions. This is for
  Shayegan's own app/digital goods only — never third-party account
  credentials (no Gmail/password pairs for Muse or any other service),
  whatever the file is named.
- Movies/Series carry a truthful "Licensed sources only" tag: the site is
  a metadata catalog and hosts/distributes no media files. A tag never
  licenses content by itself — only the rights holder or an official
  licensed service can do that.
