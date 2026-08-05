# Dad's Annual Game Bounty Hunt

A single-file, config-driven web app for running a yearly "video-game bounty hunt" —
a parent posts classic games with cash rewards; a kid captures the bounties by beating
them. Everything lives in one `index.html` with **no build step**; state is stored in the
browser (localStorage) and backed up via JSON/CSV export.

## Two modes

- **User (play)** — the default. The kid sets each game's outcome and watches the stats:
  - **Complete** — won it (confetti + "BOUNTY CLAIMED").
  - **Withdraw** — didn't get to it; can come back next year (muted card).
  - **Reject** — tried it, not for them; won't be reused ("GAME OVER / womp womp").
- **Manage (edit)** — PIN-locked (padlock button). Grown-ups can:
  - Edit the title / eyebrow / season / year / kid name / default bounty.
  - Add, edit, reorder, and delete games (up to 99). Bounties default to $25.
  - **Auto-fill** a game's details from RAWG + a Wikipedia reference link (needs a free
    RAWG key, set in Manage → Settings).
  - Import / export the board as JSON (full backup or one season) or CSV (games).
  - **Close out a season**, archiving it to the **History** tab and starting a fresh board
    (optionally carrying withdrawn games forward).

> The PIN is a speed-bump, not real security — it's stored in the browser. Take a JSON
> backup (Manage → Import / Export → Full backup) before each season as your safety net.

## Run locally

Any static server works. For example:

```bash
python3 -m http.server 4177
```

Then open <http://localhost:4177/>. You can also just open `index.html` directly
(`file://`) — only the RAWG/Wikipedia auto-fill needs a network connection.

## Deploy (Netlify + GitHub, auto-deploy on push)

1. Create the GitHub repo and push:
   ```bash
   gh repo create game-bounty-hunt --private --source=. --push
   ```
2. In Netlify, **Add new site → Import from Git**, pick the repo. Settings:
   - Build command: *(empty)*
   - Publish directory: `.`
   
   (Or run `netlify init` from this folder.) Netlify then auto-deploys on every push to
   `main`. `netlify.toml` already sets the publish dir and a `noindex` header.

## Versioning

Semver lives in `package.json` and in `APP_VERSION` inside `index.html` (shown in the
footer). To cut a release: `npm version patch` (bumps + git-tags), bump the matching
`APP_VERSION`, then `git push --follow-tags`.

## Files

- `index.html` — the whole app (HTML + CSS + JS).
- `legacy/bounty-hunt.html` — the original 2026 version, kept for reference.
- `netlify.toml`, `package.json`, `.gitignore` — deploy + tooling config.
