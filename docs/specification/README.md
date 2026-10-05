# Open Alvary specification

Version: 1.0 • Updated: 2026-10-05 • Status: target specification, with explicit implementation status

This is the canonical specification for Open Alvary's public legal-data infrastructure.
It supersedes contradictory planning statements in older documents. It does not declare
future features implemented, grant source permissions, appoint reviewers, authorize API
spending or establish a service-level agreement. The original first-deliverable brief
remains satisfied by a metadata pilot; this specification describes the complete target.

## Authority and change control

Use this package for target product behaviour; use [deployment status](../deployment-status.md)
for verified live facts, the generated schema and code for current wire behaviour, and
[DATA-LICENSE.md](../../DATA-LICENSE.md) for the metadata licence grant. When code differs
from a target requirement, record a gap; do not silently reinterpret the requirement.
Changes to a published contract require a version, migration/compatibility assessment,
updated acceptance evidence and maintainer review. Source-specific permissions remain
mandatory regardless of any product decision here.

## Current baseline

The public site is https://open.alvary.ai; API base is https://open.alvary.ai/api.
The repository is https://github.com/ruhu-ai/open-alvary. Ruhu AI maintains the project;
public contact is ijidai@ruhu.ai. Software/documentation are MIT; original metadata is
CC BY 4.0, excluding legal source texts and other excluded materials.

The live service is a manually deployed Vercel metadata snapshot: four Nigerian records,
zero cleared legal full texts, read-only API, website and verified JSONL releases.
GitHub does not deploy the website. Vercel rejects all content versions/structure nodes.
PostgreSQL/Docker is an alternative supported path, not the live operating environment.
The most recent recorded local suite passed 54 tests. Monitoring, mailbox delivery and
provider retention have not been independently verified. No funding award is claimed.

## Product boundary

Open Alvary serves researchers, legal-aid users, civic organisations, developers and
public-interest AI applications. Its core works without an LLM or commercial account.
All downstream clients, including commercial Alvary, consume the same public contracts.
Open data rights do not promise unlimited free hosted compute. Any later paid hosting
must preserve licence-compliant bulk portability and disclose limits.

Private customer documents, tenant workflows, commercial prompts, client annotations,
private libraries, generated work-product graphs, billing and editor integrations stay
outside this repository. No material enters merely because the commercial app holds it.
Restricted/licensed-only records do not appear in public search, identifiers, counts or
exports. Licensed material can enter the open layer only where its reviewed terms allow
that distribution and reuse. Third-party headnotes are not assumed to share court-text rights.

## Specification map

| File | Requirements |
|---|---|
| [Data contract](data-contract.md) | DAT: identity, document classes, legal time, representations, anchors, translations |
| [Review and ingestion](review-and-ingestion.md) | REV/ING: permissions, review states, acquisition, processing and publication |
| [Public interfaces](public-interfaces.md) | API/WEB: API evolution, citations, search, website and accessibility |
| [Releases and operations](releases-and-operations.md) | REL/OPS: immutable releases, withdrawals, hosting, monitoring and recovery |
| [Expansion and acceptance](expansion-and-acceptance.md) | EXP/ACC: Africa-wide selection, evaluation, milestones and release gates |

Each numbered requirement is normative for its target stage. “Must” describes the
acceptance obligation, not an implemented fact. Optional features are explicitly labelled.
No synthetic fixture may be presented as real law or real geographic coverage.

## Delivery stages

| Stage | Scope | Current state |
|---|---|---|
| S0 | Public metadata pilot and original software | Deployed; operational follow-up remains |
| S1 | Reliable growing catalogue, schema evolution, review records, coverage and pagination | Target |
| S2 | First reviewed full-text source, persistent review state, anchors and withdrawal drill | Target; requires qualified reviewers and source permissions |
| S3 | Additional jurisdictions/languages/document classes with per-class evidence | Target; no countries or partnerships pre-committed |
| S4 | Scale, optional semantic indexes/MCP and hosted service tiers | Optional later scope |

S2 must not be enabled in the current Vercel snapshot by removing its text prohibition.
It requires a reviewed deployment architecture providing current publication decisions,
controlled content access and withdrawal across all served revisions.
