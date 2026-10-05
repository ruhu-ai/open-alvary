# Public API v0.1

Production base URL: `https://open.alvary.ai/api`. Local base: `http://127.0.0.1:8018`.
OpenAPI is at `{base}/openapi.json`; interactive docs at `{base}/docs` may be limited
by the production CSP and are not the website developer guide. The versioned target
API is specified in [public interfaces](specification/public-interfaces.md); `/api/v1`
is future work, not a live route.
Only GET data routes are exposed. No API key, LLM or commercial subscription is needed.

| GET path | Response |
|---|---|
| /project | Configured public identity, repository link and licence status |
| /healthz | Readiness: migrated database reachable; 503 without internal details on failure |
| /jurisdictions | Jurisdiction records |
| /sources | `{total, items}`; metadata, access_mode, has_full_text, rights_status |
| /sources/{id} | Public source metadata; 404 for missing or restricted |
| /sources/{id}/versions | `{items, rights, structure, notice}`; only cleared versions |
| /search?q=... | `{total, items, scope}`; title/citation and cleared-text matching |
| /citations/{citation} | `{status, matches}`; resolved/ambiguous/unresolved |
| /coverage | Counts, jurisdiction breakdown and limitations |
| /rights/{source_id} | Public review record and computed decision |
| /releases | Available manifests and download_base |
| /releases/{date}/{jurisdiction}/{mode}/{filename} | Allowlisted file; 410 if withdrawn/invalid |

`/sources` and `/search` accept optional `jurisdiction`, `limit` (1–100), and
`offset` (0+). Search requires a nonblank `q` up to 200 characters. Matching is
case-insensitive; citation aliases normalize NFKC and whitespace. Ambiguity is
returned, not guessed away. Result order is stable source ID order from the store.
No relevance ranking, snippets from unapproved text or inferred legal conclusions.

A metadata entry is not a clearance. `has_full_text=false` means there is no version
passing every current gate. Rights evidence records are deliberately public-safe;
private legal advice must not be stored in them. A version response includes text,
source URL, retrieval timestamp, raw and canonical hashes, parser, language,
effective dates and content verification details. Empty versions are normal.

Hosting must use TLS, quotas, bounded resource limits and monitoring. v0.1 loads the
small corpus into memory per request and is not intended for public-scale traffic.
MCP and embeddings are deferred; no write tools should be added to the public API.
