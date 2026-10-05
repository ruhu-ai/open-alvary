# Initial launch candidate verification — 2026-10-05

## Completed locally

- `pytest -q`: 51 passed; one upstream Starlette/httpx deprecation warning.
- Ruff check and format checks passed.
- TypeScript/Vite production build passed, including locally bundled fonts and licence notices.
- PostgreSQL and SQLite migrations, seeding and schema checks passed.
- Production Docker images built; PostgreSQL, migration job, restricted API container
  and nginx web started successfully with isolated synthetic configuration.
- Production HTTP checks passed: homepage, health, four-source coverage, search,
  withheld text, available releases and every payload's SHA-256 hash.
- Production CSP, MIME protection, write-method rejection and runtime public identity
  configuration verified. Browser console reported no errors/warnings on inspected
  production About and Privacy pages.
- PostgreSQL custom-format backup restored into a separate test database; restored
  counts: 4 sources, 0 versions. No existing user database was touched.
- Caddy HTTPS configuration validates; no live certificate or DNS change was requested.
- Launch preflight correctly rejects missing owner/contact/domain/secret values and
  unapproved metadata licensing. Production startup runs this gate before migration.
- Public source scan found no private-key or common GitHub/OpenAI-token patterns;
  this narrow check is not a claim of exhaustive secret detection.
- Browser inspection covered the original pilot pages plus new About/contribute,
  Privacy, configured project links, and deployment-relative API examples. Responsive
  DOM check at 390px found no horizontal overflow. Saved preview:
  `artifacts/application-about-desktop.png` (ignored local artifact).

Tests cover rights denials, expired/future reviews, exact-hash approval, conditions,
attribution, independent content review, API leakage, citation ambiguity, byte anchors,
storage immutability, ingestion idempotency, parser/OCR fallback, release integrity and
withdrawal, mandatory manifest hashes, health readiness and launch configuration.

## Repository release preparation

The release candidate has been committed and pushed to the private repository
https://github.com/ruhu-ai/open-alvary. Local checks were rerun: 51 tests, Ruff and
the production web build pass. GitHub CI and public publication remain to be verified.

## Not yet complete

Public GitHub publication; real maintainer/contact verification; metadata licence
approval; chosen hosting account; live DNS/TLS; off-host backup scheduling; external
monitor/alert routing; live production smoke checks; applicant identity and submission.
A local test does not prove any of these are in place.

No Nigerian legal full text was ingested. All four seed rights records remain under
review. A concrete OCR provider and real-world legal PDF extraction are not verified
end to end. No legal opinion, grant award, institutional partnership or funding
commitment is claimed. No commercial Alvary code was copied or changed.
