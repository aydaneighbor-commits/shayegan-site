# Shayegan — Drive Library Schema

Everything lives under one Drive folder named `Shayegan`. One folder per
item. The site catalog is generated from these folders; adding an item
never requires editing site code.

```
Shayegan/
  Movies/
    <Title (Year)>/
      info.json        <- required, see template
      poster.svg       <- optional, original SVG artwork only
  Series/
    <Title (Year)>/
      info.json
      poster.svg
  Games/
    <Title (Year)>/
      info.json
  Apps/
    <Name>/
      info.json
  Projects/
    <Project Name>/
      info.json
  News/
    news.json          <- written by the daily news job, do not edit by hand
```

Rules:

- Folder name: `Title (Year)` for media, plain name for apps/projects.
- `info.json` is the single source of truth for title, year, genres,
  description and notes. Copy `info.template.json` and fill it in.
- Media entries are catalog/metadata entries. Do not put copyrighted
  video/game/app files in publicly shared folders; keep personal files
  private and unshared.
- Original AI films by Shayegan are the one exception: set
  `"license": "own-work"` and a `download_url` in `info.json`, and the
  site renders a download button. Any other `license` value never
  renders a download, whatever URL is present. Only mark work
  `own-work` when Shayegan actually created it and holds the rights
  (including the AI tool's terms for commercial use).
- No passwords, tokens, or account credentials anywhere in Drive.
  Credentials belong in Secure Vault / the hosting provider's secrets.
- The sync job reads this tree, validates every `info.json`, and writes
  `data/catalog.json` for the site. Invalid entries are skipped and
  reported, never guessed.
