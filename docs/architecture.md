# Architecture

The canonical Pydantic model is the application contract. JSON Schema is generated
from it; Alembic defines the database layout. PostgreSQL 16 is the intended shared
store; SQLite offers a small local preview. No LLM is needed for any v0.1 operation.

The source catalogue contains independently authored bibliographic facts, URLs
and a current rights record. Operator adapters fetch only explicitly approved URLs.
HTML strips common non-content elements, PDF extraction handles text layers, and
scanned pages require an injected OCR callback. Every result still needs content
verification; the generic parser is not an authority on statutory structure.

Normalized text uses NFC, LF line endings, horizontal whitespace collapse while
preserving tabs, and outer trimming. Node offsets count UTF-8 bytes, not Unicode
characters. Raw and normalized hashes have different purposes. Each version has
its own retrieval timestamp and canonical URL. Legal units reference the version's
hash and must fit within its byte span; hierarchy cycles and broken links fail.

`Store.save` uses one transaction. IDs and core source/version relationships are
relational foreign keys; variable legal metadata is a validated JSON payload.
Version content/provenance and structure are immutable on subsequent saves.
Content-review status, verifier and review date can be updated independently;
append-only review history is deferred. Changed content produces
a new version. A repeat of identical normalized input returns the existing version
without changing its retrieval timestamp. Raw-format changes with identical text
are not separately retained by this v0.1 implementation; manifestations are future work.

Public API and exports call `public_corpus`. They never serialize the operator store
directly. Rights filtering precedes text matching and citation resolution. Release
downloads recheck current rights and all hashes; do not expose the release folder
through an unguarded static server or public bucket. No-store response policy is
required for hosting; previously downloaded copies cannot be recalled.

The v0.1 database is for a small catalogue. Requests load the corpus into memory;
this is intentionally not a production-scale search backend. Before expansion,
introduce indexed public projections and per-request bounded database queries.

Read-only comparison with the existing Alvary documentation informed concepts:
source identity independent of URL, immutable versions, byte anchors, currentness
and ambiguous citation outcomes. Private knowledge, tenant entitlements, workflows
and commercial implementation were excluded. No code was copied.
