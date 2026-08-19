# Slate — public documentation site

Static HTML for the seven documents the Atlassian Marketplace links to. Generated from the
sources in the main repo by `node scripts/build-site.mjs` — **edit the sources, not these files**.

## Why a separate public repo

⛔ **Do not make the main repo public.** Its `docs/` holds the portfolio strategy, competitor
analysis, pricing rationale and deploy logs. Only this folder is meant to be public.

## Why static HTML and not a link to a hosted doc

The Marketplace link checker does not execute JavaScript. Dwell's first submission was
auto-rejected with *"Automatic Rejection - Invalid Links"* because both hosts tried return a JS
shell with none of the text in the raw response:

| URL tried | bytes | `<title>` | policy text in raw HTML |
|---|---|---|---|
| `claude.ai/code/artifact/…` | 14,062 | `Claude Artifact` | no |
| `drive.google.com/file/d/…/view` | 76,489 | `privacy-dwell.md - Google Drive` | no |

These pages return the text in the first response, so a checker can read them.

## Publish

Create an **empty public** repo on GitHub (e.g. `slate-docs`), then from this folder:

```bash
git init && git add . && git commit -m "Slate public documentation"
git branch -M main
git remote add origin https://github.com/<you>/slate-docs.git
git push -u origin main
```

Then GitHub → repo → **Settings → Pages** → Source: **Deploy from a branch** → branch `main`,
folder `/ (root)` → Save. Live at `https://<you>.github.io/slate-docs/` in a minute or two.

## Verify before pasting any URL into the listing

This is the step that was missing last time:

```bash
curl -sL https://<you>.github.io/slate-docs/privacy-dwell.html | grep -i "runs on atlassian"
```

A match means the checker can see the content. No match means do not use the URL.
