# Codex Open Source Fund — application draft

Status: prepared for applicant review; not submitted. Verify the live form immediately
before submission. Do not claim a public repository or deployed site until their links
have been verified. Applicant identity and collaborator details must come from the applicant.

Application: https://openai.com/form/codex-open-source-fund/
Checked 2026-10-05: ongoing review; awards up to $25,000 in API credits. This is not
a cash grant. The form asks for identity, project description, GitHub repository,
collaborators and API-credit use. A completed website is not stated as a prerequisite.

## Applicant fields

- First name: applicant to supply.
- Last name: applicant to supply.
- Email: ijidai@alvary.ai (applicant-selected address; confirm monitored before submission).
- LinkedIn: optional; supply only if desired.
- Personal GitHub: authenticated account observed as Jidayi; applicant to confirm.
- Project GitHub: https://github.com/ruhu-ai/open-alvary (owner-selected destination; publication/visibility still to be verified).
- Collaborators and roles: applicant to confirm; do not invent a team or list AI tools as people.

## Which open source project are you representing?

Open Alvary

## Brief description of the project

Open Alvary is an open-source, rights-aware legal-data infrastructure project for
African jurisdictions, starting with Nigeria. It aims to make authoritative legal
sources useful to researchers, legal-aid organisations and developers through
structured records, traceable provenance, a read-only API and versioned bulk releases.

Its central constraint is that public availability does not establish redistribution
rights. Full text passes a source-specific rights gate, exact-content-hash approval,
privacy clearance and independent content verification before publication. Where
permissions remain uncertain, the system exposes only independently authored
catalogue metadata and official-source links. Restricted sources stay outside the
public corpus.

The working v0.1 pilot includes canonical schemas, parser interfaces, policy checks,
a database migration, API, public website and reproducible releases. It currently
catalogues four Nigerian instruments and publishes no Nigerian legal full text;
all four rights reviews remain pending. The website and software are MIT-licensed.
Original catalogue metadata licensing is being finalized before public launch.

Open Alvary is separate from Alvary's proprietary legal-AI application. The commercial
product can consume the same public interfaces as any other downstream user. No
customer documents, commercial workflows or proprietary code are included. The
corpus remains useful without an LLM.

## How would you use API credits for your project?

We request up to $25,000 in API credits, with usage staged according to demonstrated
need, to develop and evaluate open tooling that turns African legal sources into
structured, machine-readable data for research, public-interest tools and AI applications.

Over a proposed six-month programme, we would use Codex and eligible OpenAI models to:

1. Build ingestion adapters for official HTML, text PDFs and scanned legal documents,
   beginning with Nigeria and extending to additional African jurisdictions selected
   after source and rights assessment.
2. Evaluate structured extraction and provenance, including document hierarchy,
   provisions, citations, dates and source relationships, against deterministic baselines.
3. Test multilingual processing across selected languages used in African legal sources,
   subject to qualified reviewer availability, with clear reporting of coverage and limitations.
4. Strengthen reliability and publication controls through tests covering OCR errors,
   malformed documents, citation mistakes, incomplete metadata, version conflicts and
   attempts to publish uncleared content.

We would begin with a calibration phase capped at $1,500, included within the overall
request, using original synthetic fixtures and material cleared for external API processing.
We would measure quality improvements, development costs and cost per document before
setting subsequent usage budgets. We welcome a smaller initial allocation, with further
support based on demonstrated results.

Our target public outputs include reusable ingestion tooling, evaluation code, a versioned
benchmark of at least 300 human-reviewed examples, including 60 adversarial cases,
permitted test fixtures, and published cost, performance and failure reports. Synthetic
examples would be clearly labelled and evaluated separately from real legal documents.

Model outputs would not determine publication rights or establish current law. Rights
clearance, content verification and public-release decisions would remain subject to
source-specific policies and human review. The requested amount is a maximum; actual
usage would follow evidence of useful improvements.

## Is there anything else you'd like us to know?

We have deliberately kept the initial release small and explicit about its limits.
The pilot's purpose is to demonstrate enforceable rights/provenance boundaries,
not to imply that we already operate a comprehensive legal corpus. Funding has
not been awarded, no institutional partnerships are claimed, and rights clearance
will not be outsourced to an LLM.

Proposed public outputs over 24 weeks include six tested parser/adapter improvements,
a versioned synthetic benchmark growing from 100 to at least 300 independently
checked examples, OCR and Unicode/translation-provenance fixtures, including at least 60
adversarial cases within the 300-example target, a provenance/rights regression suite, reproducible evaluation
reports and a versioned release. Multilingual fixtures do not imply that real-law
translations or new jurisdictions have already been legally or linguistically verified. Rights-cleared real content would be an additional outcome only if
permissions and content verification are completed; it is not a promised volume target.

Hosting, legal review and human annotation are separate resource needs. API credits
would not be represented as cash to pay for those activities. We welcome a smaller
initial award tied to the evaluation stage. The $25,000 request is a maximum,
not a commitment to exhaust credits: we would revise allocations after measured
pilot costs and align the schedule with any actual award expiry or restrictions.

## Evidence to attach or link after publication

- Public README, MIT licence and separate metadata licensing statement.
- Live pilot showing four metadata sources and zero cleared full texts.
- CI results and docs/verification.md; update counts after the final checks.
- docs/application/milestones-and-budget.md and docs/initial-launch-spec.md.
- Versioned JSONL manifests and rights-policy tests.

## Applicant's final review

Confirm identity, collaborator roles, publicly visible links, credit request and
project/metadata licence authority. Read the actual form's submission terms and
submit personally. The form includes an acknowledgement about OpenAI independently
developing similar projects; this draft does not accept those terms on anyone's behalf.
