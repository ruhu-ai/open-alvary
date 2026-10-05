# Open Alvary v0.1.0 — metadata catalogue pilot

Release candidate. Publication and live deployment must be recorded separately after
verification; this document does not claim either has happened.

## What this release provides

- Four independently authored Nigerian source records and official-source links.
- Search, source details, coverage, rights/governance, downloads, developer documentation,
  project/contribution information and privacy pages in a responsive public website.
- Read-only API, canonical schemas, database migrations and deterministic JSONL releases.
- Shared API/export publication checks, integrity checks and release withdrawal handling.
- Manual, HTML and text-PDF ingestion interfaces, with an explicit unconfigured OCR path.
- Docker deployment, launch preflight, CI, public smoke checks and backup/restore guidance.

The website, API and project software are MIT-licensed. Metadata terms are recorded
separately in DATA-LICENSE.md. Linked legal texts retain their own rights conditions.

## Coverage and limitations

No legal full text is included. All four sources await rights review. The 2007
Investments and Securities Act is identified as historical/repealed; other catalogue
entries do not imply a current consolidation or verified amendment history.

This is a small metadata pilot, not comprehensive Nigerian or African legal coverage.
Generic parsing creates a document root; detailed legal segmentation, production OCR,
reviewer administration, append-only rights history, MCP and large-scale indexed search
remain roadmap work. Models do not determine legal rights or automatically publish law.

## Running and deploying

Follow README.md for local use and deployment/runbook.md for a hosted pilot. A live
launch requires approved metadata terms, named ownership/contact, configured hosting,
TLS and operational monitoring/backups. Verify the deployed revision using
`python -m scripts.smoke_public https://YOUR_HOST`.

See docs/verification.md for completed checks and outstanding external steps.
