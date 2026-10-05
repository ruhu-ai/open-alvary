# Proposed six-month programme and API-credit budget

This is a proposed plan, not a record of completed work or awarded funding.
Request: up to $25,000 in API credits over 24 weeks (approximately six months).
A smaller staged award is workable. The larger request covers sustained development,
repeated evaluations and broader fixtures; it is not a claim that the current
four-record catalogue alone needs $25,000 of inference. No model-specific token-price
assumptions are used. Confirm any award expiry or usage restrictions before committing
to the schedule; a six-month usage window is proposed, not established by the form.

## Africa-wide scope and jurisdiction selection

Nigeria is the initial implementation pilot. The programme aims to build reusable
infrastructure for African jurisdictions, not to promise comprehensive continental
coverage within six months. Assess additional jurisdictions for authoritative source
availability, external-processing and redistribution permissions, document formats,
languages, and qualified reviewer capacity. Publish an assessment matrix and explicit
coverage gaps before committing to named jurisdictions or legal-text volume targets.
Blocked permissions remain documented gaps; synthetic fixtures can support tooling
but must never be represented as real jurisdiction coverage.

## Milestones and acceptance evidence

| Period | Deliverable | Acceptance evidence | Spending decision |
|---|---|---|---|
| Weeks 1–3 | Synthetic benchmark, jurisdiction assessment and deterministic baseline | At least 100 original reviewed examples; frozen held-out split, permissions inventory, cost/quota runner and measured unit costs | Calibration capped at $1,500 within the overall request |
| Weeks 4–8 | Adapter development and failure handling | Three tested improvements across manual/HTML/text-PDF paths; source assessment matrix, bounded fetching and reviewed patches | Budget set after calibration |
| Weeks 9–14 | OCR and structural parsing evaluation | Expand benchmark toward 300 human-reviewed examples; original scanned fixtures, section/citation/anchor scoring and cost report | Scale only with measured benefit and review capacity |
| Weeks 15–20 | Multilingual and adversarial regression | At least 300 total examples including 60 adversarial cases; selected-language fixtures with qualified review; six total parser/adapter improvements | Reassess actual costs and remaining useful experiments |
| Weeks 21–24 | Reproducibility and public release | Held-out comparison, permitted fixtures and scoring code, cost/failure report and next-stage recommendation | Up to $25,000 overall; no obligation to exhaust credits |

## Budget rationale

The $25,000 request is a maximum capacity request, not a measured forecast. Neither
four catalogue records nor the benchmark target alone demonstrates a need for that
amount. Previous fixed workstream allocations have been removed because no measured
usage supports them. A smaller initial award is workable.

During calibration, measure API-assisted development separately from extraction and
evaluation. Record model, input/output/cached token usage, document length, repetitions,
retry rate, actual cost and quality improvement. Forecast each next stage using the
number of authorized documents or development tasks multiplied by observed unit costs,
with explicit bounds on justified comparisons and reruns. Record human review capacity
and processing permissions alongside the forecast. Do not manufacture extra runs to
meet the requested ceiling. No paid experiment or API spending is authorized by this plan.

## Evaluation design

Use synthetic instruments clearly marked as non-law until source-specific permissions
cover external API processing, fixture redistribution and derived outputs. Metadata
licensing does not confer these rights over a linked legal document. Prevent training/
development leakage into held-out examples. Use a 70/30 development/held-out split
at each version (210/90 at 300 examples); freeze hashes before comparison and never
tune on held-out failures. Publish a new benchmark version for later development.
Report synthetic and real-source results separately if real-source evaluation becomes lawful.

Metrics: structural-node precision/recall and F1; citation match/ambiguity accuracy;
normalized character error rate; exact anchor resolution rate; publication-denial
pass rate; median/p95 latency and cost per document. Human reviewers adjudicate
expected outputs and failure categories. Record sample counts and uncertainty;
Even 300 examples cannot establish broad legal or jurisdictional accuracy.
Multilingual checks initially exercise synthetic Unicode and translation provenance;
semantic translation-quality claims require appropriately qualified reviewers.

Release invariants are strict: zero known restricted-text leakage; all published
anchors resolve; every publicly redistributed real-text version has independent
rights and content approval. Extraction accuracy targets are experimental hypotheses,
not guarantees. Choose an assisted method only when it improves the relevant baseline
without degrading the safety invariants and within the measured cost envelope.

## Budget controls

Use a dedicated OpenAI project for this public work, separate from commercial Alvary.
Confirm supported models and then-current prices at experiment start. Log request ID,
experiment ID, model/version, token usage, actual costs and permitted input classification.
Do not log confidential source text. Attribute Codex development and corpus experiments
separately. Weekly maintainer review; alerts at 50%, 75% and 90%; stop issuing requests
at the approved cap. Provider dashboard budgets must not be assumed to enforce a hard stop.
The experiment runner's quota enforcement is a milestone, not implemented in this pilot.

Proceed beyond the first $1,500 only after the benchmark and processing-rights checks
are complete and evidence supports useful further work. Measure actual development and evaluation costs in that stage, then reconcile the
next-stage budgets with observed usage. If evidence supports a lower budget,
reduce the planned spend rather than invent additional workload. Changes to scope/caps require
maintainer approval. Do not use the grant for commercial customer work or private data.

## Dependencies outside the credit request

Named maintainer; rights and content reviewers; annotation time; hosting and backups;
metadata licensing and contribution terms. None is paid for by API credits. If rights
clearance is delayed, continue synthetic benchmarks and open software improvements;
do not replace missing rights evidence with a volume commitment.
