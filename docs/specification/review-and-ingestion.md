# Review, permissions and ingestion

## REV-01 — Distinct permissions

Model permissions for discovery, acquisition, retention, external API/model processing,
full-text redistribution, metadata redistribution, quotation, derivatives, commercial
reuse and fixture publication separately. Each decision has scope, evidence, conditions,
reviewer, time, expiry/revalidation rule and affected hashes/artifact classes. Unknown
means not authorized for that operation. A public URL is not permission; software and
metadata licences never authorize linked legal text. Noncommercial/no-derivatives text
cannot enter the shared open full-text layer under the current project policy.

Metadata privacy review is separate from full-text review. Metadata must be suspendable
without inventing a full-text rights decision. A restricted source contributes no public
search result, alias, graph edge, count, snippet or release artifact.

## REV-02 — Review state and authority

Target workflow: discovered → acquisition-reviewed → acquired → parsed → validation-passed
→ content-reviewed and rights-reviewed → release-approved → published. Failure produces
quarantine with a reason and retry/review action. Suspended/withdrawn states override every
publication state. Restore requires a new recorded decision, not deletion of history.

Roles: contributor, acquisition operator, qualified rights reviewer, content reviewer,
release maintainer and incident operator. Names and capabilities are assigned explicitly;
this specification does not appoint staff. No public write API is added. A future internal
review interface requires authentication, scoped authorization, audit logging and protected
storage distinct from the public service.

Before S2, each full-text version requires separate rights and content decisions by two
distinct accountable reviewers. Automated tools may propose findings but cannot approve.
The release maintainer checks both decisions, exact hashes, conditions and expiry. A person
may hold multiple roles but cannot supply both approvals for the same version. This is a
new target control; the current pilot stores separate fields without enforcing two actors.

## REV-03 — Audit and evidence

Decisions are append-only events: event ID, actor/role, action, UTC timestamp, reason,
prior decision, source/version/hash, evidence revision and resulting status. Corrections
supersede events; they never overwrite the historical record. Public records expose only
safe reviewer identifiers and publishable summaries; privileged evidence stays private.
Authorization checks precede mutation. Concurrent approvals use revision checks to prevent
lost updates. Expired or withdrawn decisions deny publication until renewed. Audit retention
and permitted evidence retention must be recorded per evidence class before S2.

## REV-04 — Redaction and exceptions

A redacted artifact gets a new hash and explicit mapping to its original; a redaction
approval binds the actual released bytes. Verify text extraction, hidden layers, images
and metadata cannot reveal the removed content. Keep sensitive redaction rationale private.
Ambiguous conditions or privacy complaints suspend release. No emergency path skips rights
or content approval. Any operational exception must narrow access, not expand publication.

## ING-01 — Acquisition contract

Each adapter declares provider, supported formats/classes/languages, allowed URLs, permission
references, rate/concurrency limits, timeout, maximum bytes/pages, retry policy and refresh
cadence. Fetching remains operator-only. Require HTTPS; reject credentials, unexpected ports,
unapproved redirects and destinations. Enforce network egress restrictions, including private
and metadata-service networks, and revalidate resolved destinations where redirects are
supported. MIME signatures and declared type must agree or enter quarantine.

Respect source-specific access conditions. A retry uses bounded exponential backoff and
Retry-After where appropriate; permanent permission/authentication failures stop and alert.
Never discover new fetch permission from document content or model instructions.

## ING-02 — Job and storage lifecycle

Jobs record ID, adapter/config version, input reference/hash, permission revision, status,
lease/attempt, timestamps, outcome and sanitized error code. States: queued, running,
awaiting-review, succeeded, failed, cancelled. Checkpoint each stage; retries are idempotent.
A worker crash cannot publish a partially written source or release. Raw and extracted
staging are private; only reviewed projections cross into public storage.

Separate raw-object deduplication from source identity and version creation. Repeated fetches
append observations even if no new version is needed. Parser versions/configuration and
normalization profiles are reproducible. A publication transaction owns its commit and
records all linked entities together. Network/model calls do not run inside long database
transactions. Define retention/deletion for failed jobs and staging before enabling them.

## ING-03 — Parsing and quality

HTML extraction records removed/non-content regions. PDF extraction preserves page boundaries;
scans use a configured OCR provider only after processing permission is verified. Low confidence,
missing pages, unexpected language, malformed tables and empty text trigger warnings/quarantine.
Use isolated resource-bounded parsers; enforce CPU, memory, page and decompression limits.

Validate hashes, normalized text, language tags, identity references, legal hierarchy, offsets,
page mappings and deduplication before review. Extraction quality and legal correctness are
separate. Reviewer evidence must cover the selected document's tables, footnotes, schedules
and material structure, not only a clean sample paragraph.

## ING-04 — Refresh and model use

Record each refresh's reachability, items seen/changed, errors, last successful observation
and observed lag. Freshness is not legal currentness. A changed layout or repeated empty
result must not silently appear as no new law. New bytes trigger parsing/review, never automatic
legal approval. Publication remains on the last permitted revision until replaced or suspended.

Optional model experiments must log experiment/model/config version, permitted input class,
actual usage/cost and output provenance. Treat retrieved material as data, not instructions.
Use a dedicated project, enforce approved spending caps and stop on rights or budget failure.
No model is required to browse, retrieve, verify hashes or use the corpus. The proposed grant
calibration is not an authorization to spend money or send uncleared documents to a provider.
