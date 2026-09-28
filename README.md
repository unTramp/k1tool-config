# K1TOOL remote config

`config.json` is read by the K1TOOL app on start and when it returns to the
foreground. Edit it here to control updates — no app release needed.

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
2. Changes reach apps within ~5 minutes (GitHub raw cache).
3. Keep the JSON valid — the app ignores a broken file and keeps the last good
   one. Check with `python3 -m json.tool config.json`.
4. The app fails open: if this file can't be loaded and nothing is cached,
   nobody is blocked.
