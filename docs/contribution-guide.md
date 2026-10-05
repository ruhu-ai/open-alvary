# Contributing

Use Python 3.12+ and Node 20.19+. Install the pinned Python requirements and web lockfile.
Run pytest, Ruff, schema generation and the web production build before proposing a change.
CI also migrates and seeds PostgreSQL 16. Keep migrations independent of live model code.

Submit a small catalogue/adapter change with official provenance, evidence of lawful
acquisition and an honest rights status. A contributor's assertion is not a substitute
for reviewer evidence. If permission is uncertain, use RIGHTS_UNDER_REVIEW.
Never upload uncleared text, scraped compilations, customer files, credentials,
private legal advice or commercial Alvary code. Use original synthetic fixtures for tests.

Adapter contract: `fetch(source) -> bytes`, `parse(raw) -> str`. Ingestion normalizes,
hashes, creates a version/root node and persists only through the validated store.
Fetch and content review are separate operations; generic parsing never verifies law.
An OCR callback accepts original PDF bytes and a one-based page number. It must return
text or raise a clear error; subsequent human content verification is still required.

For legally meaningful structure, add a jurisdiction/source adapter that produces
validated legal nodes and anchors. Offsets are normalized UTF-8 bytes. Text changes
must generate new version and node identities.

There is no public write or review API. Initial reviewers use a controlled operator
process to write complete validated corpus records. Published records, including
rights notes and reviewer identifiers, must be suitable for public disclosure.
Contribution licence terms and reviewer ownership must be agreed before accepting
outside legal-data contributions at scale.
