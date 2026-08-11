# imbet69-ui-assets

iMBET69-only UI assets (VIP badges, avatars, payment/provider logos, category
icons, banners, promo cards, etc.) — Cloudflare Worker static assets, served
from a dedicated domain under `oraclemk.cloud`. Replaces the scattered
ImageKit URLs (5 different accounts, one of which is already hitting its
bandwidth cap) that these files used to live on.

This is intentionally a **separate repo/Worker/domain from `game-assets2`**
(which already serves game thumbnails for multiple sites, iMBET69 included).
Keeping this iMBET69-only traffic on its own Worker means a bad file/deploy
here can never affect game thumbnails on any site, and vice versa.

## Folder structure

Deterministic paths, one folder per asset category — see `MIGRATION_MAP.csv`
for the full old-URL → new-path mapping (131 files):

```
ui/
├── vip/        {level}.webp        (0-50, PromotionPage VIP badges)
├── avatar/     {n}.webp            (1-24, ProfilePage avatar picker)
├── payment/    {provider}.webp     (usdt, kbzpay, wavepay, uabpay, ayabank, kbzbank, btc)
├── provider/   {code}-logo.webp    (pg, pp, jdb, jili)
├── footer/     {label}.webp
├── category/   {name}.gif|.webp    (hot, slots, fishing, live-casino, arcade)
├── agent/      *.webp
├── promo/      {name}.webp
├── spinwheel/  *.webp|.gif
├── quicklinks/ *.webp
├── support/    *.webp
├── app/        launcher-icon.webp
├── jackpot/    chance-icon.webp
├── badge/      hot.gif
├── brand/      logo.webp
├── banner/     {n}.webp            (hero banner defaults)
├── profile/    icon-*.webp
├── referral/   *.webp
├── social/     {platform}-icon.webp
└── misc/       recent-winners-header.gif
```

## URL pattern (once the Cloudflare custom domain is attached)

```
https://cdn.oraclemk.cloud/ui/{category}/{file}
```

iMBET69 proxies this same-origin via a Vercel rewrite
(`/img/ui/(.*) → https://cdn.oraclemk.cloud/$1`), same pattern already used
for game thumbnails — so it never appears as a third-party domain in the
browser.

## How to add/update files

1. New branch off `main` — **never commit straight to `main`**.
2. Add files under `ui/...` following `MIGRATION_MAP.csv`'s `new_path`
   column exactly (case-sensitive, no spaces).
3. `.gif` files stay `.gif` — do not convert animated gifs to webp.
   Everything else -> `.webp`.
4. Open a PR. Do not merge it yourself — Kyaw Gyi or Claude reviews and
   merges. Cloudflare's GitHub integration only deploys on push to `main`,
   so a branch/PR never touches the live site.
5. Never force-push, never rewrite history, never touch `wrangler.toml`,
   folder structure, or any file outside `ui/` without asking first.

See `AGENT_PROMPT.md` for the exact spec handed to the download/convert
agent.
