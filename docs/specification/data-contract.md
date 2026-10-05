# Data contract

Status: target requirements; current Pydantic models implement only a subset.
All new entities and changes require versioned JSON Schema and migration fixtures.

## DAT-01 — Identity and provenance

A legal work has a stable opaque source ID independent of URL, provider, citation,
language, parser and hosting location. Distinguish the legal work, language expression,
canonical text version, raw manifestation, retrieval observation and publication revision.
IDs are never reused. A duplicate merge retains redirects and evidence; split/merge
operations preserve historical references and explain ambiguous mappings.

Multiple providers/URLs may represent one work. Provider-local IDs are namespaced;
URLs and citations are aliases, not global uniqueness keys. Raw-byte equality deduplicates
storage only; it must not merge distinct legal works. Text-hash equality alone must not
collapse different languages, legal effective periods or parser/structure revisions.
Record an explicit predecessor and change reason for replacement versions.

Every observation records source/provider, requested and final approved URL, UTC time,
response metadata, raw digest, media type, size, acquisition permission reference and
processing configuration. Public provenance is a safe projection; credentials, private
review evidence and privileged advice never enter public payloads or Git history.

## DAT-02 — Jurisdictions, languages and institutions

Jurisdiction IDs support national, subnational, regional and supranational scopes.
An ISO country code is optional for regional bodies; jurisdiction is not inferred from
language or URL. Parent hierarchies must be acyclic. Cross-border application is an
explicit evidenced relationship, not forced into a single country parent.

Languages use validated BCP 47 tags, including script variants. Record original language,
authoritative-language status and translation relationships separately. Institutions
have stable IDs, names/aliases, type, official URL and evidenced jurisdictions. Courts
have a reviewed hierarchy with effective periods; specialised or regional competence
must not be reduced to a universal numeric rank. Shared institutions may span jurisdictions.

## DAT-03 — Type-specific legal metadata

Every source requires title, jurisdiction scope, institution, document class, language,
canonical citation or explicit citation-unavailable state, authoritative reference,
rights/publication status and provenance. Applicable fields below must be populated or
carry an explicit unknown/not-applicable reason; absence must not be invented away.

| Class | Additional metadata and relationships |
|---|---|
| Constitutions, statutes and delegated legislation | Instrument number/year; short/official title; publication/gazette reference; enactment, assent and commencement events; parent enabling instrument; consolidation status; amendments/repeal |
| Treaties and regional instruments | Issuing body; parties/applicability evidence; adoption and entry-into-force events; protocols and reservations where reviewed |
| Judgments, opinions and orders | Court, case/docket identifiers, neutral and alternate citations, decision/publication dates, judgment/order type, appeal relationships, reportability and reviewed precedential status |
| Regulator decisions and enforcement | Regulator, decision identifier/date, enabling instrument, subject, operative order, appeal/status and privacy clearance |
| Guidance, circulars and notices | Issuer, identifier, publication/effective dates, legal basis, binding-effect evidence, replacement/withdrawal relationship |
| Gazettes and public legislative records | Issue/volume/date, publisher, constituent instrument links, page boundaries and document identity |
| Original open commentary or summaries | Authorship, publication date, licence, source references and conspicuous secondary/editorial label; never silently merged into primary text |

Case-party and personal information needs its own publication review. Reporter headnotes,
editorial annotations and summaries remain separately attributed and licensed. Private
contracts, client documents and proprietary commentary are outside the corpus.

## DAT-04 — Legal time and currentness

Record legal events independently from retrieval/review/publication timestamps. An event
has type, date or uncertain interval, precision, affected work/provisions, evidence and
review status. Support partial commencement, transitional provisions and unknown effects.
Do not assume effective intervals are closed or that publication equals commencement.

Keep separate dimensions: publication lifecycle (candidate/published/suspended/withdrawn),
legal force (in-force/partly-in-force/not-in-force/unknown), consolidation (original,
unconsolidated amendments, official consolidation, unofficial consolidation), and freshness.
Currentness is a derived, explainable result at an explicit date, using verified events.
Conflicting evidence yields a contested/uncertain result, not an arbitrary winner.
No model or successful fetch may establish current law. Preserve the evidence and algorithm
version used for any computed status. An amendment edge alone does not reconstruct text.

## DAT-05 — Representations and structure

Separate permitted raw objects, canonical text, legal hierarchy, page layout, tables,
OCR output, retrieval chunks and optional embeddings. A raw file can remain private while
an authorized derived representation is public; permissions are evaluated per artifact.
Retain raw evidence only when acquisition/retention rights permit it.

The normalization profile must be versioned: UTF-8, NFC, LF, horizontal whitespace rules
and treatment of tabs/outer trimming. Keep a mapping from extracted/layout text to canonical
text where normalization changes positions. Hash raw and canonical bytes independently.
Directly supplied canonical text must satisfy the same profile as parser-generated text.

Logical nodes support document, preamble, recital, title, part, chapter, division, article,
section, subsection, paragraph/subparagraph, schedule, annex, definition, footnote and
structured tables. Preserve labels and headings separately from stable locators. Logical
order governs navigation; visual reading order governs page display. Parent containment,
acyclicity, unique locators within a version and unambiguous sibling order are validated.
Tables retain row/column structure and merged cells; OCR records page-level confidence
and warnings. Unsupported layout remains explicitly unsupported, never silently flattened
into supposedly verified structure.

Parser correction or rechunking after publication creates a new representation/version
with a change reason. Old anchors retain their original interpretation while permitted.
Chunks reference canonical spans and nodes; optional overlapping retrieval windows must
not masquerade as distinct legal provisions. Embeddings are replaceable derived indexes,
never the only retained copy of content.

## DAT-06 — Anchors, citations and legal relationships

Canonical spans use half-open UTF-8 byte ranges [start, end) into a specified version's
normalized text. Bounds must fall on code-point boundaries and carry the content hash,
normalization-profile version and structural locator. Page/bounding-box evidence is
optional only when unavailable, with availability stated explicitly.

Public anchors have stable IDs and resolve to source, version, node/span, official URL,
display citation, permitted excerpt, attribution and review/currentness state. Distinguish
citation identity, exact quotation and explanatory paraphrase. Quote verification compares
exact normalized span bytes and a span hash; no fuzzy match is silently called exact.
If a source is withdrawn, return a safe unavailable state without leaking withheld text.

Consumer adapters must declare offset units. Byte offsets must be converted explicitly
to Unicode code points or browser UTF-16 units against the exact pinned text. Conformance
fixtures include accented text, combining marks, Arabic and supplementary characters.
Alvary's private response shapes are not the Open Alvary public contract.

Citation aliases may be ambiguous. Resolution accepts jurisdiction/type/date context and
returns candidates rather than guessing. Graph edges distinguish citation, amendment,
repeal, appeal and case treatment. Each asserted treatment links to the treating source's
exact provision/paragraph, reviewer and evidence. Unverified proposed edges are withheld
from the default public graph. Do not infer binding effect from a source's mere presence.

## DAT-07 — Translations and quality

Each translation is a separate expression/version with its own rights, hash and content
review. Record source version, target language, translator/provider, method, authoritative
status and verification evidence. The target policy withholds unverified translation
relationships from the public graph; current implementation must be changed before this
capability is advertised. Machine output is labelled and cannot be called official.
Qualified language review is required before semantic-accuracy claims. Original and translated
versions retain distinct citations/anchors and cannot silently substitute for each other.

## DAT-08 — Contract evolution

Every export and future versioned API envelope declares schema version. Breaking changes
require a new major contract, migration instructions and consumer fixtures. Additive fields
must define defaults/unknown semantics. Database migrations and public API versions need
not share numbering. Record source/representation revisions independently from schema and
embedding versions. Future indexes pin model/configuration versions; metadata-only updates
must not require changing canonical text or re-embedding unchanged content.
