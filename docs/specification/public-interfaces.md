# Public interfaces and website

## API-01 — Current and target routing

Current API: https://open.alvary.ai/api, with the paths documented in ../public-api.md.
Local development uses http://127.0.0.1:8018. The existing unversioned API remains a pilot
contract. Before S1 consumer integration, introduce `/api/v1` explicitly; this document
does not claim those routes exist now. Announce breaking migration and retain a documented
compatibility window or publish a deliberate breaking release before removing old routes.

Public data operations are read-only. No anonymous write, upload, URL-fetch, review or model
execution route is introduced. GET/HEAD must not mutate corpus state. CORS, caching and
rate limits are explicitly configured for the selected host. Optional future authentication
for hosted quotas must not alter the underlying published data licence.

## API-02 — Versioned resource contracts

| Target route under /api/v1 | Required behaviour |
|---|---|
| /project | Maintainer/contact, software/metadata terms, service stage and deployed revision |
| /jurisdictions, /authorities, /languages | Public reference records, hierarchy and supported scope |
| /sources, /sources/{id} | Safe source/type metadata, rights mode, currentness and coverage context |
| /sources/{id}/versions | Permitted versions with effective-time context, provenance and review status |
| /versions/{id}/structure | Paginated logical structure and anchor identifiers |
| /anchors/{id} | Exact permitted span, locator, hash, offset unit and attribution; safe unavailable state |
| /search | Public-only query results, total/count semantics, facets, filters and coverage notices |
| /citations/{citation} | Resolved/ambiguous/unresolved result, contextual candidates and pinpoint anchors |
| /sources/{id}/relationships | Reviewed graph edges with evidence and bounded traversal |
| /coverage | Dimensions, completeness statements, gaps, freshness and degraded status |
| /rights/{source_id} | Safe current publication decision and applicable obligations |
| /releases, /releases/{id} | Manifest, schema version, coverage and withdrawal state |
| /releases/{id}/files/{filename} | Allowlisted verified payload, current permission recheck |
| /withdrawals | Public-safe tombstones/notices where disclosure itself is permitted |
| /healthz | Minimal readiness without internals; no sensitive configuration |

Version/structure/anchor routes must recheck current eligibility, not trust earlier search
results. Hidden and nonexistent restricted identifiers have indistinguishable responses.
An openly announced withdrawal may return 410 with a safe reason; otherwise use generic
404. Tombstones preserve identity only where publication of identity remains permitted.

## API-03 — Envelopes, pagination and errors

Future list envelopes contain schema_version, items, page information, coverage notices
and deployed/dataset revision. Offset pagination remains for small pilot lists; future
cursor pagination pins dataset revision and stable ordering. A stale cursor returns an
explicit restart error, not silently mixed revisions. Default page size 25, maximum 100;
no truncation without has_more/next_cursor. Total is explicitly exact, estimated or omitted.

Required source/search filters: jurisdiction (explicit descendant option), class, authority,
language, access mode, legal status and date type/range. All filters apply before pagination.
Facets and totals are computed on the public projection and cannot reveal excluded records.
Query length remains bounded; document size and graph work also have independent bounds.

Errors use a versioned object with code, safe message, request_id, retryable and optional
field errors. Define 400 malformed query/cursor, 404 unavailable, 410 permitted withdrawal,
422 invalid input, 429 quota with Retry-After, and 503 temporary unavailable. Do not expose
stack traces, private URLs, database details or source text in errors. Unknown rights states
fail closed. Current implementation's FastAPI errors are not yet this target contract.

## API-04 — Search and authority resolution

Separate exact citation lookup, provision lookup and general lexical search. Case-folding,
Unicode normalization and jurisdiction-specific citation normalization are versioned and
tested. Preserve ambiguous aliases. Date filters must identify decision/publication/effective
semantics explicitly. Default result ordering and tie-breakers are documented and stable.

General search can later combine lexical and optional semantic indexes, but semantic scores
cannot establish legal authority/currentness. Ranking must consider supported jurisdiction,
source class, reviewed authority and freshness without inventing missing metadata. Explain
coverage warnings and label secondary/editorial sources. Snippets are derived only from
permitted spans and obey quote/attribution conditions; metadata-only sources yield no text.

Graph defaults: depth 1, maximum depth 3, at most 100 edges per page; enforce a visited set
and a global work budget. Pending or unverified treatment edges are excluded by default.
“No result” must be distinguishable from unsupported scope, incomplete coverage or degraded
retrieval. Optional embeddings must be reproducible, replaceable and rights-filtered before
candidate text is exposed; post-ranking filtering alone is insufficient.

## API-05 — Consumer compatibility and optional MCP

Publish schemas, examples and independent conformance fixtures for citations, anchors,
normalization, rights modes, ambiguity, currentness and withdrawal. An adapter into Alvary
must map explicit offset units and preserve stable source/version IDs and unavailable states.
It may add private document links downstream, never write those back to the public corpus.

A future MCP layer wraps the same read-only contracts and decisions, with bounded inputs,
outputs and traversal. It does not gain separate privileged access, accept arbitrary URLs,
or execute instructions embedded in retrieved text. Model-specific indexes and prompts are
not required by the corpus contract.

## WEB-01 — User journeys

1. Discover: home → jurisdiction/coverage → browse/search → source. Show actual scope and
   metadata/full-text status at every stage; preserve filters/query/page in shareable URLs.
2. Inspect: source → rights/provenance → official reference or permitted version. Show
   currentness uncertainty, source type, reviewer status and corrections contact.
3. Read: version → table of contents → provision/paragraph/page anchor. Stable deep links,
   version selector, effective-date context, attribution and exact copy/citation actions.
   Never silently substitute a newer text at an old anchor.
4. Reuse: developer guide → schema/API examples → release manifest → verified download.
   Include metadata terms, source-specific text obligations, revision and withdrawal notices.
5. Correct/contribute: visible reporting path, safe issue guidance, no public upload of
   confidential evidence; explain review status and contribution licences.

Browse must expose all matching records, apply access filters server-side and display
meaningful empty/loading/error states. Unsupported routes/invalid encodings must not crash
the app. Network failures are recoverable. No account or commercial subscription is required
for the base public catalogue. Accessibility and privacy information are easy to locate.

## WEB-02 — Accessibility, languages and privacy

Target WCAG 2.2 AA for core journeys, verified through keyboard, screen-reader, contrast,
zoom/reflow and touch-target checks. Do not claim compliance solely from automated scans.
Test 320px through desktop widths, 200% zoom, focus restoration, accessible menus/forms,
error announcements, reduced motion and bidirectional text. URLs/citation labels must remain
usable in right-to-left layouts. Original content language is distinct from UI locale.

No third-party analytics, advertising or document uploads in the pilot. Bundle fonts locally.
Describe actual host logging, retention, search processing and external links. Never promise
that provider infrastructure logs nothing. Contact data is used only for the stated enquiry;
retention and access responsibility must be documented. CSP must match deployed assets.
