# Slate — public documentation site

Static HTML for the documents the Atlassian Marketplace links to, plus the commercial pages
buyers arrive on from search. Generated from sources in the main repo by
`node scripts/build-site.mjs` — **edit the sources, not these files**.

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

The repo already exists at `github.com/attrify/slate-public`, checked out separately at
`/home/bee/sln/slate-public`. From the main repo:

```bash
node scripts/build-site.mjs                       # regenerate site/
cp site/*.html site/*.css site/*.txt site/*.xml site/README.md site/.nojekyll ../slate-public/
cd ../slate-public && git add -A && git commit -m "..." && git push
```

⚠️ **Pages fails silently.** On 2026-08-20 three consecutive deploys failed and the privacy page
the listing points at served a stale copy for 40 minutes. Always check
`github.com/attrify/slate-public/actions` is green **before** trusting the URLs.

## Verify before pasting any URL into the listing

This is the step that was missing last time:

```bash
curl -sL https://attrify.github.io/slate-public/privacy-dwell.html | grep -i "runs on atlassian"
```

A match means the checker can see the content. No match means do not use the URL.
