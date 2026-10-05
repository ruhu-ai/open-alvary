# Expansion, acceptance and delivery

## EXP-01 — Africa-wide selection

Nigeria is the first demonstrated catalogue, not a proxy for continental coverage. Maintain
a candidate matrix with jurisdiction/body, document class, language/script, official sources,
API/acquisition/retention/redistribution permissions, formats/volume, legal hierarchy,
reviewer availability, update cadence, demand and estimated operating cost. National,
subnational and regional materials are considered explicitly. Score candidates transparently;
no country or source is approved simply because it appears in a commercial product.

Select one bounded source collection per new jurisdiction first. Entry gate: authoritative
source, known permission constraints, accountable acquisition/review roles, supported format
and per-class fixtures. Exit gate: reproducible ingestion, verified metadata/anchors, rights
checks, visible coverage gaps and sustainable refresh operation. A blocked source remains
metadata/link-only if that is permitted; it is never silently replaced with an unreviewed mirror.
Do not pre-commit additional countries, partners or language reviewers without evidence.

## EXP-02 — Coverage records

Describe jurisdiction, authority/court, class, period, language, expected universe where known,
observed count, collection method, last check/success, cadence, known gaps and review state.
Completeness values: sample, partial, substantial, complete, unknown; complete requires an
identified authoritative denominator and reconciliation evidence. Never compute completeness
from a raw count alone. Freshness values distinguish on-schedule, stale, degraded and unknown.
Coverage notices appear in API search and relevant website journeys.

## ACC-01 — Traceability and evidence matrix

| Requirement group | Required acceptance evidence | Stage |
|---|---|---|
| DAT-01/02/08 | ID merge/split, hierarchy-cycle and language-tag rejection; schema migration/compatibility fixtures | S1 |
| DAT-03/04 | Per-class required/unknown fields; partial commencement, conflicting evidence and historical-date fixtures | S2 per supported class |
| DAT-05/06 | Raw/text dedupe separation; page/table/footnote preservation; exact spans and multibyte offset conversions | S2 |
| DAT-07 | Translation permissions, review gating, language labels and provenance fixtures | S3 before translations |
| REV-01–04 | Operation-specific denial matrix; two-actor approval; expiry, redaction, suspension and append-only history | S2; metadata withdrawal in S1 |
| ING-01–04 | Network/size/parser isolation; retry/crash recovery; idempotency; source-change and low-quality quarantine | S2 |
| API-01–05 | Public schemas; permission-before-pagination; ambiguity; cursor revisions; typed errors; independent consumer fixtures | S1/S2 by endpoint |
| WEB-01/02 | Browse >50 records/search >25 matches; full-text navigation fixture; keyboard/screen-reader/RTL/zoom checks | S1 catalogue; S2 reader; S3 language |
| REL-01/02 | Reproducible build; same-day revisions; tamper rejection; withdrawal across aliases and rollback | S1/S2 |
| OPS-01–05 | Anonymous smoke; monitor/alert proof; backup restore; capacity report; deployment and incident runbooks | S1; persistent-store drills before S2 |
| EXP-01/02 | Source-selection evidence and reconciled coverage/freshness reports | Before each S3 expansion |

Evidence names the tested commit/configuration, fixture hashes, reviewer, date, observed
result and limitations. Passing synthetic tests does not establish real-source quality.
All requirements need an owner and work item before entering an implementation milestone;
this package does not invent assignees or delivery dates.

## ACC-02 — Evaluation design

Use independently authored synthetic fixtures until external processing and redistribution
permissions are verified. Separate synthetic and real-source results. Freeze a development/
held-out split by source family, not just random pages, to avoid near-duplicate leakage.
Version fixtures and gold annotations; keep a never-tuned held-out set for each reported run.
Classify failures and require qualified adjudication of legal/language-specific outputs.

The proposed funding target is at least 300 human-reviewed examples, including at least 60
adversarial cases, not 360 total. Report allocation by class/language/format and exclude
unreviewed generated examples from that count. No dataset of that size is claimed to exist.
Adversarial cases include missing rights, expiry, changed hashes, hidden/redacted text,
malformed PDF, uncertain citations, broken anchors, stale versions and prompt injection.

Metrics: metadata accuracy by field, structure precision/recall, OCR character error rate,
exact quote/anchor success, citation ambiguity handling, retrieval relevance, currentness
label support, language coverage, cost per task and latency. Rights-denial and integrity
invariants require 100% pass on the defined suite and zero known publication bypasses.
Extraction/ranking thresholds must be set per supported class during calibration and frozen
before held-out evaluation; no universal accuracy claim is inferred from 300 examples.

## ACC-03 — Stage release gates

S0 facts are recorded in deployment-status.md; remaining operational gaps are not erased by
having launched. S1 closes documentation drift, implements catalogue growth behaviour and
schema/version discipline, exercises metadata withdrawal and records monitoring ownership.
S2 requires persistent publication authority, permitted source material, distinct reviewers,
verified content/structure, stable anchors, attribution and a successful withdrawal/restore
exercise. Enabling text requires a separate implementation/release decision; not just a flag.
S3 repeats type/language/jurisdiction acceptance for each new collection and publishes coverage.
S4 is optional and follows measured need; embeddings/MCP/paid hosting are not prerequisites.

## ACC-04 — Funding and unresolved external dependencies

The requested up-to-$25,000 credits and proposed six months are a staged programme, not a
measured spending forecast or award term. Calibration is capped at $1,500 within that request;
subsequent budgets depend on measured unit costs and useful work. API credits do not fund
human review, hosting or legal clearance. See ../application/milestones-and-budget.md.

External decisions that cannot honestly be specified as facts: source permission outcomes,
qualified reviewer appointments, country/language priorities, hosting spend limits, mailbox
operation, on-call availability, legally appropriate retention and grant expiry. Each must
be recorded with owner/evidence before the affected gate passes. These dependencies do not
prevent implementing the explicit schema/API/test contracts above.
