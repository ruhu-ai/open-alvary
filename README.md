# Open Alvary

A rights-aware, machine-readable public legal corpus for African jurisdictions.
Nigeria is the first catalogue. Open Alvary is shared public data infrastructure;
**Alvary is a separate proprietary legal-AI product** that may consume the same
public interfaces as researchers, legal-aid organisations and other developers.
No commercial Alvary code, customer material, private annotations, prompts,
playbooks, or proprietary workflows are included here.

## v0.1 reality

Four independently authored Nigerian catalogue records; **zero cleared full-text
sources**. Every seed rights record is `RIGHTS_UNDER_REVIEW`. The release contains
metadata and official links, not legal text. Public accessibility is not a licence.
The 2007 Investments and Securities Act is marked historical/repealed using the
[SEC Acts page](https://www.sec.gov.ng/our-mandate/regulation/acts/). No current
consolidation or complete amendment coverage is asserted for the other entries.

## Run locally

Python 3.12+ and Node 20.19+ (Node 22 recommended). Commands run from this repository.

```sh
python3.12 -m venv .venv
. .venv/bin/activate
pip install -r requirements.lock
pip install --no-deps -e .
alembic upgrade head
python -m scripts.seed
uvicorn api.main:app --host 127.0.0.1 --port 8018
```

In another terminal:

```sh
npm --prefix web ci
npm --prefix web run dev
```

Open http://127.0.0.1:5178 and API docs at http://127.0.0.1:8018/docs.
SQLite is the no-service development default. For PostgreSQL, set `DATABASE_URL`
before running migrations, seeding and the API. `.env.example` is a template;
environment files are not automatically loaded.

Alternatively, `docker compose up --build` starts PostgreSQL 16, the API and the
built web app on the same ports. Stop native preview processes first. Compose
credentials are for local development only. The database has no published port.
No deployment to open.alvary.ai is performed by this repository.

## Repository tree

```text
schema/              Pydantic canonical contracts + generated JSON Schema
rights/              enums (in schema), fail-closed policy, public projection
ingestion/          adapter contract, parsers, normalization, ingestion pipeline
   catalogue/ng.json four metadata candidates; no imported legal text
api/                 FastAPI GET endpoints + SQLAlchemy store
migrations/          Alembic initial migration (PostgreSQL / SQLite)
exports/             immutable JSONL release builder + serving-time verification
releases/2026-10-05/ng/
   corpus/           rights-filtered release (metadata only in this pilot)
   metadata/         explicit metadata-only release
web/                 React + TypeScript + Vite public website
scripts/             seed, schema generation, database verification
tests/              policy, leakage, provenance, parser and release tests
docs/                architecture, governance, policies, API, roadmap
.github/workflows/   tests, schema drift, PostgreSQL migration and web build
```

## Architecture and database

`catalogue → approved fetch → parse → normalize → rights decision → structure →
validate → persist in operator database → gated API/export`

Canonical models describe jurisdictions, authorities, languages, sources, rights
records, immutable source versions, hierarchical legal nodes, citation aliases,
amendment/cross-reference relationships and translation provenance. Tables retain
relational identities and core foreign keys, with a validated JSON payload for
extensible domain attributes. Secondary graph links and byte anchors are checked
by the corpus validator. This is a small-corpus store; production indexed search,
streaming projections and audit history are future work.

Raw-file and normalized-text SHA-256 values are separate. Nodes refer to source,
version, structural locator, optional page and UTF-8 byte offsets. Generic ingestion
creates a document root; jurisdiction-specific legal segmentation is not claimed.

Compatible *concepts* were informed by the existing Alvary schema documentation:
stable source identity, immutable versions, normalized UTF-8 anchors, explicit
currentness and ambiguous citation resolution. All implementation is new. No
proprietary code was copied.

## Rights decision flow

1. Missing record or `RESTRICTED` → no public record.
2. Explicit metadata publication basis → catalogue facts and official link only.
3. `OPEN` / `OPEN_WITH_CONDITIONS` is necessary but insufficient for full text.
4. Require redistribution, commercial reuse and derivative permissions; reviewer,
   date, licence, holder, evidence and legal/database-rights notes; privacy clearance;
   attribution text where required; and satisfied recorded conditions.
5. Require approval of the **exact normalized content hash**, plus separate content
   verification of that version. Future-dated/expired reviews fail closed.
6. Apply the same projection before API search, retrieval, citations and every export.
   Serving a release also checks hashes and current rights; withdrawn, changed or
   invalid releases return HTTP 410 and disappear from the listing.

Rights records are public-facing records. Store privileged legal advice and private
reviewer contact details elsewhere. A licence review must ensure all declared
conditions can actually be fulfilled by the release format and downstream users.

## API

All public operations are GET:

- `/jurisdictions`
- `/sources` and `/sources/{id}`
- `/sources/{id}/versions` (cleared text and nodes only)
- `/search?q=...`
- `/citations/{citation}` (resolved / ambiguous / unresolved)
- `/coverage`
- `/rights/{source_id}`
- `/releases`
- `/releases/{date}/{jurisdiction}/{mode}/{filename}` (allowlisted artifacts)

Source lists and search accept `jurisdiction`, `limit` (maximum 100), and `offset`.
See [public API](docs/public-api.md) and the generated `/openapi.json`.

## Releases and verification

```sh
pytest -q
ruff check .
ruff format --check .
python -m scripts.generate_schema
npm --prefix web run build
python -m exports.release --date 2026-10-06 --jurisdiction ng
python -m exports.release --date 2026-10-06 --jurisdiction ng --metadata-only
```

Each immutable release contains JSONL entity files, coverage, rights summary,
README and a SHA-256 manifest. Existing releases are never overwritten. Empty
versions/structure files in the pilot are intentional. Synthetic text exists only
in tests; it is not Nigerian law and is never part of the seed release.

## Implemented, limited and deferred

Implemented: schemas and reference checks; rights/publication gate; Nigeria
catalogue; manual/HTML/text-PDF parsing; OCR callback with explicit failure when
unconfigured; persistence and migrations; nine requested read-only endpoints;
search/citation resolution; responsive public pages; deterministic releases and tests.

Limited: generic root-only structure, operator-only ingestion, in-memory substring
search over database rows, one active rights record per source, hash-based version
deduplication. PostgreSQL is supported, while SQLite powers the lightweight preview.

Deferred: live source crawling, concrete OCR service, legal section parsers,
reviewer/admin workflow, append-only rights audit, incremental search indexes,
embeddings/pgvector, MCP, hosted tiers, signed releases, authentication and per-client quotas,
production hosting and an operational takedown/contact channel. A shared API traffic
limit, deployment health checks and production configuration are now provided.

## Before public launch

Appoint rights and content reviewers; verify source-specific redistribution and
reuse evidence, editorial/database restrictions, privacy, attribution and licence
compatibility. Verify actual official document versions, consolidations, effective
dates, repeal relationships and extraction quality. Select a licence for original
catalogue metadata and agree contribution terms. Establish governance ownership,
a takedown channel, incident handling, monitoring, backups and controlled publication.
No seed's legal-text rights have been verified. See [rights policy](docs/rights-policy.md).

## Licensing

The newly written software is provided under the MIT licence in `LICENSE`.
That licence **does not apply to legal source text, external publications, source
permissions or logos**. Data permissions are per record. The pilot does not assign
a blanket open-data licence to its catalogue or any linked legal text; metadata
licensing must be settled before public distribution.

## Application-ready pilot work

The [initial launch specification](docs/initial-launch-spec.md) defines the first
public pilot and its acceptance gates. The [deployment runbook](deployment/runbook.md)
covers HTTPS, generated secrets, health checks, traffic limits, backup/restore and
withdrawal. The website adds About/contribute and Privacy pages; public identity is
read from `GET /project` and deployment environment values, never guessed.

A [fund application draft](docs/application/codex-open-source-fund.md),
[proposed milestones/budget](docs/application/milestones-and-budget.md) and
[readiness checklist](docs/application/readiness.md) are prepared for owner review.
No application has been submitted. The selected GitHub destination is
`ruhu-ai/open-alvary`; public visibility, hosting, maintainer/contact and the
original-metadata licence must be verified before launch. Full-text legal clearance
is not claimed and is not required to demonstrate this metadata-only pilot.

The [v0.1.0 release notes](docs/release-notes-v0.1.0.md) describe the release candidate,
its coverage and known limitations.
