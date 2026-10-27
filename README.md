# pattern-desk-feed

Display-only Pattern Desk overlay feed. Masked account numbers only. Not a trading endpoint.

## CORS-safe browser URLs (required for Pattern Desk)

GitHub `raw.githubusercontent.com` and `api.github.com` break browser fetches that send a `Cache-Control` request header:

- `api.github.com` preflight succeeds but does **not** allow `cache-control` in `Access-Control-Allow-Headers`
- `raw.githubusercontent.com` OPTIONS preflight returns **403**

**Use jsDelivr** (mirrors this public repo; CORS OK including `cache-control` preflight):

- `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/tradier_positions.json`
- `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/tradier-overlay.json`
- `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/robinhood-overlay.json`
- `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/production-snapshot.json`

Smallest app-side alternatives (either works):

1. Fetch the jsDelivr URLs above, or
2. Keep raw.githubusercontent.com but **drop** the `Cache-Control` request header (simple GET already returns `Access-Control-Allow-Origin: *`).

Feed publish path is unchanged (`FEED_GITHUB_TOKEN` → Contents API). Ranking/grades untouched.

## Field contract: `week_calendar` (do not silently revert)

`production-snapshot.json` → `week_calendar` is an **array of date-prefixed strings**, typically ~40–60 lines for a busy earnings week. Example shape:

```json
"week_calendar": [
  "Tue 10/27: FOMC day 1 + earnings barrage: ...",
  "Tue 10/27: BA Q3 earnings",
  "Tue 10/27: GLW Q3 earnings",
  "Wed 10/28: MSFT FQ1 FY27 earnings",
  "Wed 10/28: GOOGL Q3 earnings"
]
```

### Publisher / feed rules (GHB + Session Tape)

- **Keep** one string per event/ticker/theme with a leading date key (`Tue 10/27:`, `Wed 10/28:`, …).
- **Never** replace the array with a single combined prose blurb (or a handful of day summaries) as the only market calendar field.
- Thematic/macro blurbs may coexist as extra same-date lines; they must not replace per-ticker earnings lines.
- `calendar_events` may be empty; the desk market strip is driven by `week_calendar`.

### Pattern Desk Build rules (UI)

- Group lines **by date prefix** (text before the first `:`).
- **Default view must list every same-day line** (or at minimum every earnings/event ticker line). Do **not** hide the ticker list behind `+N` / expand-only.
- Compact day headers are OK (e.g. `Tue 10/27 · 10 items`); the body under each day must show the full ticker/event list without a click.
- Expanding a day may still reveal thematic blurbs if you want a two-tier layout, but earnings tickers must already be visible when collapsed is off / default expanded.
- Do **not** map the market strip to only `week_calendar[0]` or a joined short summary string.

This contract exists because the desk previously reverted to a one-line “combined short explanation + +N” default even while the feed still had full per-day ticker lines.
