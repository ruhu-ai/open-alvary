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

PostgreSQL migration `0002` adds identity shadow tables and a transactional maintenance
fence. Legacy records remain authoritative; migration does not backfill automatically,
publish text, or enable a target writer. SQLite retains its pilot schema. The additional
immutable DTO contract is `schema/identity.schema.json` (`identity-1`); current HTTP
payloads and source URLs are unchanged.

Maintenance helpers in `api.identity` accept an existing transaction and never commit
for their caller. In a synthetic test database, run `backfill(connection, expected_epoch=0)`,
commit, then `freeze(connection, expected_epoch=0)` and commit before final backfill and
reconciliation at epoch 1. `rehearse_cutover` rechecks full record/policy/hash parity and
denies legacy serving; it enables no target writer. `end_rehearsal` returns to frozen,
then `resume_legacy` can end this pre-cutover exercise. These are development helpers;
authenticated production review and live cutover remain unimplemented.

The fence covers INSERT/UPDATE/DELETE/TRUNCATE on every legacy table, including direct
SQL, and waits for active writer transactions. Runtime roles must lack migration-state/
identity-table writes, table ownership, schema CREATE and superuser privileges. Restricted
synthetic roles exercise the fence; compose still uses owner credentials, so production
role provisioning remains outstanding. Backfill takes an exclusive state-row lock for
the bounded copy; measure lock time before real migration. Failed copies roll back.
Downgrade is allowed only in legacy phase and removes shadows, preserving pilot data.
After a future target writer starts, use forward repair or compatible application rollback;
never resume serving stale legacy policy.

PostgreSQL migration `0003` adds separate `policy`, `staging` and `corpus` schemas
(isolated child schemas in test namespaces). It seeds no decisions, actors or approvals.
Legacy metadata handlers and Vercel remain unchanged; these foundations do not activate
new public routes or full text. `schema/policy.schema.json` versions the private record
shapes and the small public metadata/result DTOs separately from pilot responses.

Role adoption is an explicit administrator operation, not a startup hook:
`rights.database.provision_roles(connection, PolicyNamespace(base_schema))`, inside a
caller-owned transaction. It creates NOLOGIN, non-superuser serving/acquisition/review/
release capability groups and a private function/view guard. It refuses existing role
names so grants must be inspected before reprovisioning. The migration administrator
needs role-creation and object-ownership authority; those privileges must never be given
to runtime users. Group membership is granted to individual database subjects separately.
The policy foundation currently accepts **synthetic identities only**; real provisioning,
qualification and authenticated command workflows remain future work. Existing compose
credentials are still owner credentials, not a deployment of these capability boundaries.

Serving has only an eligible metadata view and narrow Boolean policy functions; it cannot
read private evidence, raw staging, legacy tables or mutate decisions. Review access uses
forced row-level security and explicit collection assignments. Actor checks bind to the
database session subject, so switching an inherited role cannot impersonate a reviewer.
History is append-only; each revision and its audit event commit together. Version
overrides can only narrow collection grants. Private acquisition pins assessment/privacy/
controller revisions and bounded retention, independently of public permission. Expired
or superseded authority hides staging immediately; scoped erasure removes bytes through
a narrow audited function. The metadata view checks current policy on each statement;
final emission checks, restore/journal guarantees and real publication remain unimplemented.

`PolicyRepository` uses caller-owned transactions, bounded metadata lookups and revision
locks. Its approval preflight checks distinct current reviewers and all six required
material coverage records; it does not mint approved text versions. The guard owns narrow
functions/view, never private tables, and has no schema CREATE after provisioning. Runtime
groups cannot assume it. Nonempty policy foundations require forward repair; downgrade
will not discard decisions or evidence. Measure migration/lock costs before real use.

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
Use the generated `/openapi.json` for the deployed API contract.

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
a persistent full-text deployment and exercised incident/withdrawal operations.
The metadata site is live on Vercel; contact is ijidai@ruhu.ai. Docker traffic controls
are available for that alternative profile, not the live Vercel deployment.

## Before full-text publication

Appoint rights and content reviewers; verify source-specific redistribution and
reuse evidence, editorial/database restrictions, privacy, attribution and licence
compatibility. Verify actual official document versions, consolidations, effective
dates, repeal relationships and extraction quality. Original metadata terms and
maintainer identity are approved. Complete review appointments, incident exercises,
monitoring, recovery and controlled full-text publication.
No seed's legal-text rights have been verified. Legal texts require documented source-specific permission and review before release.

## Licensing

The newly written software is provided under the MIT licence in `LICENSE`.
That licence **does not apply to legal source text, external publications, source
permissions or logos**. Data permissions are per record. Original project-authored catalogue metadata is CC BY 4.0 under
[DATA-LICENSE.md](DATA-LICENSE.md). This does not license linked legal texts.

## Pilot status

The public metadata site is https://open.alvary.ai. Full-text legal clearance is not
claimed. The [deployment runbook](deployment/runbook.md) covers deployment and operational
checks. Funding has not been awarded; planned capabilities are not deployed features.

## Manual Vercel deployment

Follow [the Vercel guide](deployment/vercel.md) to deploy the website and API together
from the repository root. This target is restricted to the immutable metadata pilot;
it requires no separate database. Production builds require recorded metadata licence
approval and configured public identity. Live hosting is not implied by configuration.

## Repository scope

This public repository contains the pilot implementation, public setup instructions,
licences and deployment guides. Internal design/specification documents and their
planning-only validation tools are maintained locally and excluded from public commits.

## Persistent configuration and PostgreSQL tests

Container profiles set `ALVARY_RUNTIME_MODE=persistent`. This requires an explicit
`DATABASE_URL` using `postgresql+psycopg` with a database name; it never falls back to
SQLite. The default `development` mode retains the pilot's local SQLite workflow.
The Vercel entrypoint remains an independent, metadata-only in-memory snapshot.

Run real database tests against a **dedicated disposable database whose name ends
in `_test`**. Its harness user needs permission to create schemas and to create,
assume and remove generated NOLOGIN roles for fence tests (the disposable PostgreSQL
container and CI administrator provide this). Application assertions run under those
restricted roles. Each test removes its generated schema/roles; never supply a production URL.

```sh
export TEST_DATABASE_URL='postgresql+psycopg://test_user:test_password@localhost:5432/open_alvary_test'
pytest -q -m postgres --run-postgres
```

CI requires this suite. Without `--run-postgres`, local runs explicitly skip these
tests; with the flag, missing or invalid test configuration fails. SQLite tests do
not establish PostgreSQL behaviour. The populated-upgrade tests exercise 0001 → 0002
with existing records. Restricted-role fence tests cover the migration boundary;
production policy role isolation and authenticated approval concurrency remain future work.

The `open_alvary.requests` logger emits JSON request events when INFO logging is enabled:
server-generated request ID, allowlisted HTTP method, status and time to response headers.
`X-Request-ID` correlates successful HTTP responses with these events. Request paths, query
strings, bodies, client addresses, credentials and exception text are not included by this
logger. Transport/server logging is separate; the production profile disables access logs.
This is diagnostic correlation, not an authenticated actor audit or delivery-byte receipt.
`/healthz` checks database access to the corpus table; it does not certify source clearance,
complete migration compatibility, backups or publication readiness.
