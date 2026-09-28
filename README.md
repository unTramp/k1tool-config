# K1TOOL remote config

`config.json` is read by the K1TOOL app on start and when it returns to the
foreground, via GitHub Pages: https://untramp.github.io/k1tool-config/config.json Edit it here to control updates — no app release needed.

| Field | Effect |
|---|---|
| `ios/android.min_build` | builds **below** it see a blocking *Update required* screen |
| `ios/android.latest_build` | builds below it see a dismissible *New version available* banner |
| `ios/android.store_url` | where the *Update* button leads |
| `maintenance.enabled` | shows a *Maintenance* screen to everyone (e.g. when the pool API is down) |
| `maintenance.message`, `update_message` | optional texts (`en`, `ru`); empty = built-in text |

Build numbers are the part after `+` in the app's `pubspec.yaml`
(`2.0.0+200` → `200`).

**Rules**

1. Raise `min_build` only **after** the new build is live in the store,
   otherwise users are sent to a store page without the update.
2. Changes reach apps within 1–2 minutes (GitHub Pages redeploys on every commit).
   Builds released before 2.0.0 (200) don't read this file at all.
3. Keep the JSON valid — the app ignores a broken file and keeps the last good
   one. Check with `python3 -m json.tool config.json`.
4. The app fails open: if this file can't be loaded and nothing is cached,
   nobody is blocked.

## Promo banner

`promo` shows one campaign card on top of Portfolio (builds 200+):

| Field | |
|---|---|
| `id` | campaign id; closing is remembered per id — use a new id for a new campaign |
| `enabled` | `true` to show |
| `platforms` / `languages` | optional lists, e.g. `["ios"]`, `["ru"]`; missing = everyone |
| `starts_at` / `ends_at` | optional ISO dates, e.g. `2026-10-01T00:00:00Z` |
| `title`, `text`, `cta`, `label` | `{ "ru": "…", "en": "…" }`; only `title` is required |
| `url` | https link opened in the browser |
| `image_url` | optional square https image (≥ 144 px), e.g. hosted in this repo |
| `dismissible` | `false` hides the close button |
| `erid` | ad token (required for ads in Russia), shown as «Реклама · erid: …» |

To stop a campaign set `"enabled": false`.
