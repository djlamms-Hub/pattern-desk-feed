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
