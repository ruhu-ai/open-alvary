# Initial public pilot specification

Version: 0.1 launch candidate. This document governs the application-ready metadata
pilot, not the eventual full African legal corpus. Status labels below distinguish
implemented behaviour, required operating decisions, and explicitly deferred work.

## Purpose and audience

Researchers and legal-aid users should discover an official legal source and understand
whether Open Alvary holds cleared text. Developers should inspect the schema, query
public records and verify releases. Fund reviewers should see actual open software,
a reproducible pilot, clear limitations and a specific proposed use of API credits.

Do not imply comprehensive coverage, current consolidated law, grant acceptance,
third-party partnerships, independent legal clearance or a populated full-text corpus.
The first catalogue contains four Nigerian records and no legal full text.

## Public surfaces and acceptance criteria

| Surface | Required behaviour | Evidence |
|---|---|---|
| Home | Purpose, open/commercial separation, pilot label, live counts, search | Browser + /coverage |
| Browse | Title/citation matching, access filter, empty/error states, source links | Browser + API tests |
| Reader | Metadata, official link, rights/currentness, withheld-text explanation; cleared versions carry attribution | Policy/API tests + browser |
| Coverage | Catalogue/full-text counts and explicit jurisdiction/content gaps | /coverage + browser |
| Governance | Three access modes, source-specific rights gate, correction and withdrawal policy | Policy docs + tests |
| Releases | JSONL/downloads, manifest hashes, modes/counts, no static bypass of rights gate | Release tests + deployed smoke |
| Developers | Actual site-relative API base, endpoints, schemas, GET-only boundary and future-work labels | API/build + browser |
| About/contribute | MIT software, separate metadata licence, maintainer, monitored contact, real repository link | Owner configuration + live checks |
| Privacy/use | No accounts/uploads/analytics, search processing, external links, logging limits, contact | Config review + browser |

Keyboard users can reach navigation/search/downloads, use a skip link and see focus.
The mobile menu exposes expanded state and closes after route changes. Layouts must
remain usable at 390px and desktop widths. This is a scoped accessibility check,
not a claim of certified WCAG compliance. Never render raw source HTML as trusted UI.

## Data and API contract

Canonical definitions are schema/models.py and generated corpus.schema.json. Stable
IDs identify sources independently of URLs; versions retain canonical URL, retrieval
time, raw and normalized SHA-256 hashes. NFC/LF normalized text is anchored with UTF-8
byte offsets; structure integrity is validated. The initial parser emits a document
root, not inferred legal sections. Citation aliases may legitimately be ambiguous.

The database has relational core identities/FKs plus validated JSON payloads. Corpus
validation covers secondary references. One transaction owns saves. Content/provenance
and structure are immutable; current review status is mutable. An append-only review
ledger is required before routine multi-reviewer operations, but deferred from the
metadata pilot because it approves and publishes no real legal text.

Public routes: jurisdictions, source list/detail/versions, search, citation resolution,
coverage, rights, releases/downloads, project information and health. Lists are bounded
at 100 items per response. Search is substring matching over the small corpus loaded
into memory; do not describe it as a scalable relevance engine. Health checks require
both database connectivity and a migrated source table, expose no exception details,
and return 503 on database failure. No write, upload, review or model API is public.

## Publication and licensing invariants

Missing/restricted rights deny public visibility. Unknown/link-only rights may expose
independently authored metadata only with recorded basis. Open text requires the exact
approved content hash; rights holder/licence/evidence/dates; reuse/derivative permissions;
resolved conditions/attribution; privacy clearance and separate content verification.
All API and export paths share the gate. Downloads validate file hashes and current
rights; changed or withdrawn records suspend stale bundles. Public full-text count
must remain zero until human approvals exist. AI cannot approve rights or currentness.

MIT applies to software/project documentation. Original catalogue metadata has its
own owner-approved licence. Neither licence grants rights over official legal texts,
external compilations, private data or trademarks. Record data-licence decisions in
DATA-LICENSE.md. Public configuration alone is not licence authority.

## Deployment contract

Production composition separates database, one-shot migration, API, nginx web and
Caddy HTTPS edge. Only ports 80/443 are public. Database/API are internal. Generate a
unique password; do not reuse local demo credentials. Runtime API filesystem is read-only,
capabilities dropped, process concurrency bounded, container memory/log rotation capped.
nginx restricts methods and applies a shared 20-request/second API budget with burst 40.
This is a small-service load guard, not a complete abuse/DDoS solution or per-user quota.

Caddy obtains TLS after DNS and inbound 80/443 are correctly configured. Hosting-specific
firewall, uptime monitor, provider logging/retention and backup destination must be
confirmed by the operator. `/api/healthz` is the monitor target. Alert recipient is the
named maintainer; no external monitor or alert channel is silently assumed active.

Fonts are bundled; production CSP disallows external scripts, fonts, embedding and
arbitrary connections. Do not apply this production CSP to Vite development HMR.
Access logging is disabled; operational logs rotate. No tracking analytics are installed.
Backup database daily with deployment/backup.sh; encrypt an off-host copy and restore
a test database before launch. See deployment/runbook.md for rollback and withdrawal.

## Operational responsibilities

The maintainer owns releases, access to hosting, incident response and funding reports.
The public contact must be tested by the owner. A credible rights/privacy complaint
triggers triage; suspend affected source visibility/releases pending review. Never
request sensitive evidence in public GitHub issues. Corrections receive new versions
or updated review decisions; unknown legal effects remain unknown.

No service-level guarantee is promised by the pilot. A named legal reviewer is necessary
before full-text launch, not a fictional role filled to make a form look complete.
No external funding commitments, partnerships or team membership may be invented.

## Launch gates

1. Tests, formatting, schema drift, web build and clean-container smoke all pass.
2. Source tree reviewed for secrets/private/proprietary material and generated noise.
3. Owner chooses public repository destination and publication is verified anonymously.
4. Maintainer/contact and metadata licence approved; preflight passes with real values.
5. Deploy to approved host/domain; verify TLS, links, API, download hash and empty text.
6. Database backup/restore exercise and external monitoring/alert ownership recorded.
7. Application copy matches actual status; applicant reviews identity/budget/terms.

The application can be submitted without a deployed website if the applicant chooses;
the official form does not list a completed site as a prerequisite. Never mark an
unmet public-launch gate complete merely to strengthen an application.

## Explicitly outside this first launch

Real-law volume targets; commercial/private databases; legally meaningful section
parsers for every jurisdiction; concrete OCR service; translations; automated currentness;
reviewer/admin UI; audit ledger; scalable search; embeddings; MCP; paid tiers; anonymous
uploads; production legal advice. The funded milestones develop public engineering
capabilities under controlled rights/processing permissions, not a substitute for law review.
