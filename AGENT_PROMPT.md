# Task: migrate 131 iMBET69 UI images off ImageKit → imbet69-ui-assets repo

## 1. What to download
Every row in `MIGRATION_MAP.csv` in this repo. Columns: `category`,
`new_path`, `imagekit_url`, `used_in`. Download the exact URL in
`imagekit_url` for every row — 131 rows total.

## 2. Convert to webp — except gifs
Convert every downloaded file to `.webp`, **except** files where `new_path`
already ends in `.gif` — those are animated and must stay `.gif` exactly as
downloaded, do not touch/re-encode them.

## 3. File naming and placement
Save each file at the exact path in the `new_path` column, relative to the
repo root (e.g. `new_path = ui/vip/5.webp` → save to `ui/vip/5.webp` in this
repo). Do not rename, do not flatten folders, do not change case.

## 4. Git workflow — read this carefully, this is a production-adjacent repo
- Clone `ccorryxx-bot/imbet69-ui-assets`.
- Create a **new branch** off `main`, e.g. `agent/imagekit-migration`.
  **Never commit directly to `main`.**
- Add only the 131 files above, each at its exact `new_path`. Do not modify,
  delete, or move any existing file (`wrangler.toml`, `README.md`,
  `MIGRATION_MAP.csv`, `.gitignore`, etc.) — new files only.
- Commit normally (no `--amend`), push the branch, **open a pull request**
  into `main`. **Do not merge the PR.** Kyaw Gyi or Claude reviews and
  merges it manually.
- **Never force-push** (`git push --force` / `-f`), never rewrite history
  (`rebase -i`, `filter-branch`), never delete branches other than your own
  working branch.
- This repo is wired to auto-deploy to a live Cloudflare Worker on push to
  `main`. Because you're only ever pushing to a feature branch and opening
  a PR, nothing you do can reach production without a human merging it —
  stay on that branch/PR workflow and that safety holds automatically.

## 5. If anything is ambiguous or a download fails
Skip that row, note it, keep going — don't guess. List any skipped/failed
rows in the PR description along with the reason (404, redirect, wrong
content-type, etc.) so Kyaw Gyi/Claude can follow up individually.
