# pattern-desk-feed

Display-only Pattern Desk overlay feed. Masked account numbers only. Not a trading endpoint.

## CORS-safe browser URLs (required)

GitHub `raw.githubusercontent.com` and `api.github.com` break browser fetches that send a `Cache-Control` request header (preflight fails: raw OPTIONS 403; api.github.com does not allow `cache-control` in `Access-Control-Allow-Headers`).

Use **jsDelivr** (already mirrors this public repo; full CORS including `cache-control` preflight):

- Positions badges: `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/tradier_positions.json`
- Tradier overlay: `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/tradier-overlay.json`
- Robinhood overlay: `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/robinhood-overlay.json`
- Production snapshot: `https://cdn.jsdelivr.net/gh/djlamms-Hub/pattern-desk-feed@main/production-snapshot.json`

Alternate (no custom request headers): plain GET to `https://raw.githubusercontent.com/djlamms-Hub/pattern-desk-feed/main/<file>` works (`Access-Control-Allow-Origin: *`) only if the client does **not** set `Cache-Control` / other non-simple headers.

GitHub Pages (`https://djlamms-hub.github.io/pattern-desk-feed/`) is enabled as a static mirror but is **not** a substitute for jsDelivr when the client sends `Cache-Control` (Pages OPTIONS returns 405).
