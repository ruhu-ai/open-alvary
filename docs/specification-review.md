# Specification review — 2026-10-05

> Review snapshot before specification reconciliation. Its findings are addressed as
> target requirements in [the canonical specification](specification/README.md); that
> does not mean the identified implementation gaps have been fixed.

## Assessment and scope

The repository substantially satisfies the attached brief's **first deliverable**:
a rights-aware metadata pilot, schemas, ingestion skeleton, API, website, sample
releases and tests. It does not yet specify or implement a comprehensive African
legal corpus. The distinction is intentional in the original brief, which asks for
a small first deliverable rather than all eventual capabilities.

This review compares that brief with repository documentation, implementation and
the recorded public deployment. It is an engineering/product review, not a legal
opinion or a new source-permissions assessment. No production settings or corpus
records were changed. Future targets below are recommendations, not approved delivery
commitments. Deployment evidence is in deployment-status.md.

## Current baseline

- Public website: https://open.alvary.ai; public API base: https://open.alvary.ai/api.
- Public repository: https://github.com/ruhu-ai/open-alvary.
- Maintainer: Ruhu AI; approved public contact: ijidai@ruhu.ai. Mailbox response
  or delivery has not been independently tested.
- MIT software/documentation; CC BY 4.0 for original catalogue metadata, with explicit
  exclusions for legal texts and other third-party material.
- Four Nigerian catalogue records; no published legal full text.
- Manual Vercel deployments, no GitHub deployment integration.
- Each Vercel instance reconstructs an in-memory metadata database from the deployed
  snapshot. It rejects versions and legal structure nodes, even if otherwise cleared.
- PostgreSQL/Docker is a separate supported deployment path, not the current live stack.

## Requirement coverage

| Area | Current state | Remaining specification needed |
|---|---|---|
| Open/commercial separation | Explicit in code scope, site and docs | Governance authority, contribution disputes, long-term stewardship |
| Rights and provenance | Strong publication gate; exact hashes; separate review fields | Auditable decisions, reviewer authority, processing permission and metadata withdrawal rules |
| Canonical data | Core entities and reference checks implemented | Legal timelines, manifestations, ID lifecycle, stronger hierarchy/language validation |
| Ingestion | Manual, HTML and text-PDF paths; OCR callback | Production adapters, job lifecycle, parser isolation, OCR quality and source refresh |
| Legal structure | Node schema supports hierarchy and byte anchors | Actual legal segmentation, page maps, acceptance fixtures and stable deep links |
| API | Required read-only routes implemented | Compatibility/versioning, error contract, filtering, pagination and scale targets |
| Public website | Requested pages and metadata workflows implemented | Growth pagination, multilingual navigation, full-text reader and accessibility acceptance |
| Releases | Deterministic JSONL, hashes and rights checks | Manifest/schema versions, same-day revisions, correction/withdrawal semantics |
| Africa-wide scope | Extensible entities and selection principles | Country/language selection matrix, reviewer capacity, phased coverage targets |
| Operations | Public HTTPS deployment and smoke checks verified | Monitoring, alerts, platform limits, incident and withdrawal exercises |
| Fund programme | Staged calibration and deliverable targets proposed | Measured unit costs, annotation capacity and validated next-stage budgets |

## Findings and recommended requirements

### 1. Reconcile specifications with the public deployment — immediate

The initial launch specification's deployment contract describes PostgreSQL, nginx,
Caddy, container limits and daily database backups. Those controls do not describe
the live Vercel snapshot. Architecture describes SQLite as local-only; public-api.md
claims no hosted endpoint; roadmap.md calls this a local foundation; readiness.md
still marks contact, licence, repository and deployment as pending. governance.md
also treats metadata licensing as unsettled. Earlier verification documents are useful
historical evidence but should be explicitly dated and linked to current status.

**Acceptance:** one current status page; separate Vercel and Docker operating contracts;
correct live API examples and licence/contact facts; historic checks distinguished
from active controls. Do not mark uptime monitoring, mailbox delivery or off-host
backup arrangements complete without evidence.

### 2. Define and exercise withdrawal on Vercel — immediate

Current-rights checks use each deployment's snapshot. Redeploying a correction does
not mutate old deployment URLs or already cloned public Git history. The Vercel guide
acknowledges this; the general governance/release specifications sound more immediate.
This matters even for metadata corrections and becomes critical for future text.

**Acceptance:** inventory public aliases and protected previews; identify the operator
who can suspend distribution; test a synthetic withdrawal; verify the current domain
and superseded deployment URLs; define rollback safeguards, notices and mirror policy.
Do not claim downloaded or Git-cloned copies can be recalled. Future mutable text
publication needs a current shared authority or equivalent revocation mechanism.

### 3. Make growth behaviour explicit in the website — before catalogue expansion

SourceList fetches a single default API page and filters its returned items locally.
Browse defaults to 50 rows, search to 25. There are no next-page controls. Beyond those
sizes users can miss records, and an access filter can appear empty when matching
records exist on later pages. The four-record pilot is unaffected.

**Acceptance:** server-side jurisdiction/access filtering, consistent totals and
pagination; a fixture exceeding both limits; test that every matching source is
reachable. Define which filters belong in the URL and how empty/error states behave.

### 4. Specify reviewer authority and decision history — before full text

Rights and content checks occupy separate fields, but neither stores an append-only
review ledger or authenticates reviewer authority. The gate accepts the same reviewer
identifier in both roles. Thus “independent” currently means separate review records,
not an enforced two-person rule. Decide which interpretation is required.

**Acceptance:** explicit roles, permitted transitions, evidence revision, rejection,
expiry and supersession rules; actor/time/reason on every decision; approval bound to
hashes; tests that unauthorized transitions fail. If two-person review is selected,
enforce different accountable actors. Define metadata-specific privacy/withdrawal
rules too; full-text expiry/privacy checks do not automatically hide approved metadata.

### 5. Model legal time and source manifestations — before current-law claims

Versions have effective_from/effective_until and sources have a coarse legal status.
There is no explicit distinction between enactment, publication, commencement,
partial commencement, consolidation-as-of date, observation date and review date.
Amendments connect whole sources rather than individual affected provisions.
Identical normalized text deduplicates even if a newly fetched raw file differs.

**Acceptance:** define legal document identity, language expressions and raw
manifestations; preserve repeated retrieval evidence; represent unknown/partial legal
effects without guessing; specify provision-level amendment links and point-in-time
queries before advertising historical/current consolidated law.

### 6. Separate acquisition and external-processing permission — before automation

The publication record is detailed, while permission to fetch, retain, send to an
external model, redistribute fixtures and publish derived outputs is documented mainly
as operator procedure. The ingestion function does not implement these distinct
permission decisions. An HTTPS URL allowlist is an acquisition control, not evidence
of all those permissions.

**Acceptance:** explicit permission/evidence records for each operation, enforcement
before the operation, approved storage locations and retention, bounded retries,
content-type verification, parser/OCR resource limits, job status and reproducible
parser/configuration versions. Keep restricted network egress as an operating control.

### 7. Define translation quality and publication semantics — before multilingual text

Translation provenance includes method and a verified flag, but public_corpus includes
an unverified translation relationship when both linked versions are publishable.
This is not hidden text leakage: both texts still need full-text approval. It is an
unsettled quality-policy question about publishing the relationship itself.

**Acceptance:** choose whether unverified relationships are withheld or visibly labelled;
record review evidence and reviewer; distinguish official, human and machine versions;
define per-language quality criteria, directionality, search behaviour and coverage gaps.
Never infer semantic translation quality from Unicode tests alone.

### 8. Strengthen schema invariants — before broader data contributions

Jurisdiction parent references are checked for existence but not cycles. A jurisdiction
can currently be its own parent. Language.tag is a nonempty string with a BCP 47 comment,
not validated BCP 47. Authority/jurisdiction compatibility, duplicate locators and sibling
ordering need explicit policy rather than blanket constraints that might reject valid
cross-jurisdiction authorities.

**Acceptance:** specify supported hierarchies and identifiers, prohibit invalid cycles,
validate language tags, define exceptions explicitly and add rejection fixtures for
impossible relationships. Version the schema and define compatibility/migration policy.

### 9. Specify the full-text reader separately — before enabling text

The current reader can render text and hashes but lacks a legal table of contents,
version comparison/selection, effective-date context and navigable provision anchors.
The schema's ability to hold structured nodes does not constitute that user experience.

**Acceptance:** an approved fixture with parts/sections/schedules, stable citation links,
source/page provenance, attribution, version/currentness labels, keyboard navigation,
copy/export behaviour and a visible correction path. Handle invalid routes and missing
versions as recoverable states. Broaden accessibility testing beyond viewport fit.

### 10. Establish API/release compatibility and operating targets — before scale

The API has bounded response lists but loads the whole corpus. Release verification
reads payloads on requests. No numeric corpus/traffic envelope or performance SLO is
specified. Date-only releases cannot express an in-place same-day revision; putting
builds in separate roots does not create distinct public API release IDs.

**Acceptance:** versioned contracts and manifests; stable ordering/pagination guarantees;
structured errors; release revision identifiers; deterministic verification and withdrawal
behaviour; measured latency/concurrency envelope; provider traffic/spend limits and
external alert routing. Choose targets from measurements, not arbitrary promises.

### 11. Turn continental ambition into selectable work packages — before commitments

No additional countries, languages, document volumes or reviewers are confirmed.
That is honest, but insufficient for a detailed multi-jurisdiction delivery contract.
The 300 human-reviewed examples and 60 adversarial cases are future targets, not data
already produced or staffing already secured.

**Acceptance:** source-selection matrix covering demand, authoritative availability,
permissions, formats, legal systems, languages and reviewer capacity; ranked candidates;
one defined initial document class per selected jurisdiction; entry/exit criteria and
fallback when permissions fail. Keep Nigeria as the proven pilot and describe later
coverage as proposed until those checks succeed.

## Checks performed for this review

Read the original attached brief, specifications, policies, budget, deployment guide,
models, publication policy, ingestion pipeline, Vercel entrypoint, API and relevant UI.
Ran isolated synthetic reproductions (no production mutation):

- Corpus validation accepts a jurisdiction whose parent_id equals its own id.
- A fully cleared synthetic version remains publishable with the same reviewer ID
  in rights and content review fields.
- An unverified translation relationship remains public when both synthetic versions
  pass the content/rights gate.

These last two observations need policy decisions; they are not proof of disclosure
of uncleared text. The current Vercel deployment prohibits text versions entirely.
The test suite was not rerun as part of this review; the latest recorded run passed 54.

## Recommended order

1. Reconcile current documentation, API examples and active deployment controls.
2. Exercise withdrawals, confirm monitoring/contact operation, and define incident ownership.
3. Specify review state/history, legal time and acquisition/processing permissions.
4. Define one end-to-end reviewed source workflow and its acceptance fixtures.
5. Add catalogue pagination and the legal reader before increasing content volume.
6. Select further jurisdictions/languages using evidence, then run the cost calibration.

No change to application budget, legal clearance, hosted service guarantees or delivery
scope is approved merely by this review.
