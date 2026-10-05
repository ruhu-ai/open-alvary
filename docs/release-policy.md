# Release policy

Release paths are `releases/YYYY-MM-DD/jurisdiction/{corpus,metadata}/`.
Date IDs use the actual build date; do not backdate a new release. Versioned
exports are immutable. To correct one, create a new release and withdraw the old
one from distribution as needed. Multiple builds in one day can use separate
operator output roots; semver revision IDs are future work.

The builder validates the complete corpus and applies the shared rights gate,
then jurisdiction filtering. JSONL rows sort by stable ID; field ordering is
stable and Unicode is UTF-8. No runtime timestamp is embedded in the manifest,
so identical input and current eligibility produce identical bytes.

Every release includes jurisdiction/authority/language reference tables, sources,
versions, structure, citations, rights, amendments, translations, coverage,
rights summary and README. The manifest hashes every payload file. The manifest
is not cryptographically signed; signatures and external attestation are deferred.

The sample is metadata-only in substance: four candidates and zero cleared texts.
It deliberately includes empty versions and structure files. Test fixtures are
never release content. No licence is inferred from file inclusion or a source URL.

The API verifies hashes and current publication eligibility before serving any
artifact. Changed rights, withdrawn sources or unapproved released versions suspend
the whole bundle with HTTP 410. The listing omits suspended bundles. Do not mount
releases as public static files or put private staging into a web root. Previous
downloads cannot be technically recalled; publish withdrawal notices before launch.
