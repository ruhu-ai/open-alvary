# Public deployment — 2026-10-05

Website: https://open.alvary.ai
Alternate production address: https://open-alvary.vercel.app
Public source repository: https://github.com/ruhu-ai/open-alvary
Deployed source commit: 8bbe597

Deployed manually through Vercel CLI. There is no GitHub deployment integration;
Git-triggered deployments are disabled. Preview URLs remain protected.

Anonymous HTTPS requests to both production addresses returned HTTP 200 without a
login redirect. The public smoke check passed: homepage, API health, four catalogue
records, zero cleared texts, search, withheld text and every release payload hash.
GET /api/project reports Ruhu AI, ijidai@ruhu.ai, MIT software and CC-BY-4.0 metadata.
All 54 local tests passed before deployment; Vercel's production build succeeded.

This remains the immutable metadata-only pilot. No legal full text is published.
External uptime monitoring, provider log retention and a routine operational review
schedule have not been verified. See deployment/vercel.md for snapshot withdrawal
and manual redeployment procedures. No funding application has been submitted.
