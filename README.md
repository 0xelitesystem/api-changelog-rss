# api-changelog-rss

Paste two OpenAPI specs, get a structured changelog: added, removed, changed, breaking. Export as Markdown. Single HTML file, browser-only.

**Live demo:** https://0xelitesystem.github.io/api-changelog-rss/

## Why

Every API team that updates a spec needs to communicate what changed to consumers. Most teams either skip this entirely (bad) or maintain it by hand (slow and error-prone). This tool reads the structural changes and produces the changelog automatically.

The repo name says "RSS" because that was the original ambition (auto-watch a spec URL and emit a feed); the simpler tool ships first. RSS-from-URL would require a backend fetcher; this in-browser version covers the manual diff use case.

## Use it

Open `index.html` in any browser. Or visit `https://0xelitesystem.github.io/api-changelog-rss/` once Pages is enabled.

1. Paste the old spec (JSON or YAML) on the left
2. Paste the new spec on the right
3. Click "Generate diff"
4. See the structured report with breaking changes called out
5. Click "Export as Markdown" to get a paste-ready changelog

Or click "Load sample" to see a worked example with a Pet Store API going from v1 to v2.

## What gets detected

**Added:**
- New paths
- New methods on existing paths

**Removed:**
- Removed paths
- Removed methods

**Changed:**
- Added parameters (with required vs optional flagged)
- Removed parameters
- Parameters becoming required (was optional)
- Parameter type changes
- Request body added, removed, or made required
- Added/removed response codes

**Breaking changes** are flagged separately:
- Endpoint removed
- Required parameter added
- Parameter removed
- Parameter type changed
- 2xx response code removed
- Request body became required

The "breaking" classification is a heuristic; review the flagged items before trusting them in a release announcement.

## Markdown export

The export produces a structured Markdown changelog with sections for breaking, added, removed, and changed. Drop this directly into a CHANGELOG.md, a release note, or a Slack message.

Example output for the bundled sample:

```markdown
# Pet Store Changelog: v1.0.0 to v2.0.0

## Breaking changes

- `POST /pets` Clients calling this endpoint will receive 404.
- `GET /pets` query param `limit` is now required
- `GET /pets/{petId}` path param `petId` type: string -> integer

## Added (2)

- `DELETE /pets/{petId}` Delete a pet
- `GET /owners` List all owners

## Removed (1)

- `POST /pets` Create a pet

## Changed (2)

- `GET /pets`
  - **BREAKING:** query param `limit` is now required
  - Added query param `tag` (optional)
- `GET /pets/{petId}`
  - **BREAKING:** path param `petId` type: string -> integer
```

## Tech

- Single HTML file
- Vanilla JS, no frameworks, no dependencies
- JSON parses with `JSON.parse`
- YAML parses with a small bundled parser supporting the common OpenAPI subset (mappings, sequences, scalars, multi-line strings). For complex YAML with anchors or merge keys, convert to JSON first
- Light and dark themes with newspaper / wire-service aesthetic
- WCAG AA contrast on both themes

## What it doesn't do

- Doesn't fetch specs from URLs. (That would require a server. Paste manually for now.)
- Doesn't generate an RSS feed. (Same reason. The repo name reflects ambition, not current scope.)
- Doesn't handle OpenAPI 2.0 (Swagger). 3.x only.
- Doesn't dive into nested schema diffs (request/response body shape changes beyond presence). For schema-level diffing, look at `oasdiff` or `openapi-diff`.
- Doesn't audit for spec validity. If your specs aren't valid OpenAPI, results are undefined.

## License

MIT. See [LICENSE](LICENSE).

## Related

- [json-to-typescript](https://github.com/0xelitesystem/json-to-typescript), generate TypeScript types from JSON
- [csv-to-anything](https://github.com/0xelitesystem/csv-to-anything), convert CSV to JSON/SQL/Markdown/TS
- [webhook-inspector](https://github.com/0xelitesystem/webhook-inspector), inspect incoming webhook payloads
