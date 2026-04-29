# CRE Deal Analyzer — Backup Repository

This repository stores **dated snapshots** of the CRE Underwriting Calculator HTML that
runs at [crecentral.com/underwriting](https://crecentral.com/underwriting).

The live tool itself is hosted in a GoHighLevel custom code block — this repo exists so
Tyler always has a clean, line-preserved backup he can recover from if anything ever breaks.

---

## How to Find the Latest Version

Files in this repo are named with the ISO date and pass number:

```
index-YYYY-MM-DD-passXX.html
```

**The newest file (highest date, highest pass number) is the most recent version of the
tool.** Sort the file list by name in descending order to find it quickly.

For example: `index-2026-04-28-pass82.html` is the April 28, 2026 deployment of Pass 82.

---

## Why Dated Snapshots, Not a Single `index.html`?

Each deploy creates a permanent, immutable snapshot. This protects against:

- Accidentally clobbering a known-good version with a broken update
- Losing the ability to roll back to a specific pass without digging through git history
- Confusion about "is this the latest?" — every file's date is in its name

If you need to see how the tool changed between two versions, just open the two dated files
side by side. No git diff archaeology required.

---

## Recovery

If the live tool at `crecentral.com/underwriting` breaks (white screen, login won't work,
"Members Only" fallback page appears, etc.):

1. Open the file list in this repository
2. Click the most recent dated `index-*.html` file
3. Click the **Raw** button (top-right of the file viewer) to see the unrendered source
4. Cmd+A → Cmd+C to copy the entire file
5. Open GoHighLevel → page editor for `crecentral.com/underwriting`
6. Delete the existing custom code block entirely (don't just edit — GHL caches old content)
7. Add a new custom code block, paste the copied HTML, save
8. Hard-refresh the live page (Cmd+Shift+R or Ctrl+Shift+R)

The most common cause of needing recovery is the "newline-apocalypse bug" — see the
underwriting-tool-updater skill file for full diagnostic details.

---

## Naming Convention

Standard format:
```
index-YYYY-MM-DD-passXX.html
```

For multiple deploys on the same day, append a short description after the pass number:
```
index-2026-04-28-pass82-init-fix.html
index-2026-04-28-pass83-feature-name.html
```

ISO date format (`YYYY-MM-DD`) ensures files sort chronologically by name, so the newest
version is always at the bottom of an alphabetically-sorted list.

---

## Related Repositories

The server-side API endpoints live separately at
[`github.com/tylercauble630/cre-calculator-api`](https://github.com/tylercauble630/cre-calculator-api).
That repo auto-deploys to Vercel on every push to `main`.

The HTML files in **this** repo call those API endpoints for:
- Authentication (`/api/verify-member`)
- Calculations (`/api/calculate`)
- Deal save/load (`/api/deals`, `/api/save-deal`)
- Usage tracking (`/api/track`)
- AI Assistant chat (`/api/assistant`)

When debugging tool issues, both repos may need attention — the HTML side handles UI and
client-side calculations for sensitivity matrices, while the API side handles the
authoritative dashboard math, deal storage, and security.

---

## Convention Note (April 28, 2026)

This repo previously used `index.html` as the "always-latest" source of truth. The convention
has shifted to dated snapshots only — `index.html` (if present) may be stale or out of date.
**Always trust the most recent dated file**, not `index.html`.
