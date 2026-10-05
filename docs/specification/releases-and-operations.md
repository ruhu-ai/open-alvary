# Releases and operations

## REL-01 — Immutable, portable releases

Current paths use date/jurisdiction/mode. The target introduces unique revision IDs such as
`2026-10-05.1`; a new correction increments the revision, never overwrites bytes. Schema
version, dataset revision, software revision and legal effective date are different fields.
Each manifest records release ID, schema/profile versions, creation time supplied as fixed
build input, builder revision, jurisdiction/scope, parent release, licence references,
coverage, file byte sizes/counts and SHA-256 digests. Determinism means identical declared
inputs produce identical bytes, including timestamps. Stable UTF-8 serialization and sort
order are part of the contract. Do not put secrets or private storage paths in manifests.

Build from a validated snapshot. Verify references, permissions, review approvals, attribution
and every payload digest. Stage artifacts privately, then activate a complete release atomically.
Interrupted builds remain unavailable. Serving any payload rechecks current publication
eligibility; invalid or withdrawn bundles are absent from listings. Releases never bypass
checks via static buckets/CDN paths. A signed-manifest scheme is optional until a documented
key owner, rotation and verification procedure exist; hashes alone are not signatures.

## REL-02 — Corrections and withdrawals

Distinguish metadata correction, parser correction, legal change, rights withdrawal and
privacy incident. Publish a replacement revision with a safe explanation where permitted.
Old snapshots may remain historically useful only while their continued publication is
allowed. A changed rights assertion invalidates stale bundles. Define whether each metadata
correction needs a withdrawal rather than assuming all historical metadata must disappear.

Withdrawal workflow: receive/report → record private evidence → assess affected identities
and artifacts → suspend distribution → verify all public routes/aliases → publish safe notice
→ review/remediate → issue replacement or retain withdrawal. Keep an audit of actions and
verification. Purge affected caches/index projections and disable old public deployments.
Rollback must consult the withdrawal list and cannot restore disallowed content. Previously
downloaded, forked or mirrored material cannot be technically recalled; provide machine-readable
notices for cooperative consumers without exposing newly restricted identifiers.

## OPS-01 — Live Vercel profile

Manual CLI uploads only; no GitHub deployment integration. The public production domain is
open.alvary.ai. Previews stay protected. Each function reconstructs a read-only in-memory
metadata snapshot. No database migration, persistent write, background crawler or legal text
is supported there. The code must continue rejecting text versions and structure nodes.

Record deployed commit, deployment ID, aliases, environment configuration revision and smoke
results. Release changes require a manual reviewed redeploy. Old deployments have independent
snapshots: removal from the latest catalogue alone is not a global takedown. Repository history
is the source record, but establish a recoverable independent repository backup before claiming
resilience. Public Git history must never contain private staging or sensitive evidence.

Vercel controls replace Docker operational controls: verify platform logging/retention,
traffic/spend protections and deployment protection. Do not claim nginx rate limits, Caddy
configuration, container log rotation or PostgreSQL backups operate on Vercel.

## OPS-02 — Persistent corpus profile for S2

Use a durable shared publication/review store, private permitted raw-object storage and
isolated ingestion workers. Separate public read-only serving credentials from operators.
Docker/PostgreSQL is one provided implementation path; another host requires equivalent
controls and an architecture decision. Run migrations explicitly with backup and rollback
assessment. No automatic seed process may overwrite later reviewer decisions.

Encrypt transport, restrict database/network access, scope credentials, rotate them on
incident and exclude secrets from Git/logs. Audit administrative mutations. Restore and
withdrawal procedures must be tested before text publication. Record storage region and
source-specific retention constraints; this specification makes no blanket residency claim.

## OPS-03 — Observability and incidents

Before declaring S1 operational, configure an external five-minute check of homepage and
API readiness, alert after two consecutive failures, and name a primary and backup operator.
This is a proposed operating target, not an existing monitor or customer SLA. Record daily
backup health for persistent stores and source-refresh failures against each approved cadence.
Track error/latency rates, denied releases, job failures, stale coverage, bytes and provider
spending without logging confidential queries/text or private evidence.

Rights/privacy exposure takes priority over availability: suspend the affected distribution
while investigating. Other incidents distinguish downtime, data corruption, compromised
credentials and stale sources. Record detection, response, communication and recovery evidence.
A business-hours/on-call coverage arrangement and response commitments must be chosen by the
maintainer before any SLA is advertised. Public contact configuration is not a mailbox test.

## OPS-04 — Recovery and capacity

S0 recovery: rebuild a reviewed repository revision, restore public configuration, redeploy
and run anonymous smoke checks. Confirm the rollback revision contains no withdrawn records.
S2 target: daily encrypted off-host database/object backups, proposed 24-hour RPO and one-working-day
RTO, quarterly isolated restore drill. These targets require staffing/provider confirmation;
no promise is made until a measured drill succeeds. Retention is set per rights/privacy class.

Before catalogue growth, publish a tested capacity envelope: record count, text bytes, request
mix, concurrency, p50/p95 latency, memory and cost. Initial S1 benchmark target is 10,000 synthetic
metadata records, 10 concurrent clients and p95 under two seconds for warm search/detail requests;
report cold-start separately. This is an acceptance target to validate, not current performance.
If it fails, optimize or narrow the advertised envelope before increasing coverage. Do not load
an unbounded text corpus or rehash large releases synchronously on every request at scale.

## OPS-05 — Release security and verification

Use least-privilege CI credentials, dependency/secret scanning, locked reproducible builds,
private vulnerability reporting, safe filename/path handling and resource-limited parsing.
Verify GET-only public data operations, no restricted-text leakage, no static release bypass,
appropriate response headers and anonymous production access. Tests must cover fresh installs,
migrations, snapshot bootstrap, recovery and expiry/withdrawal races. Frontend assets can use
content-hash caching; rights-bearing API responses remain no-store unless a revocation-aware
cache design is separately approved and tested.
