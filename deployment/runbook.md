# Public pilot deployment runbook

Use a Linux host with Docker Compose, persistent storage, and sufficient memory
(start with at least 2 GiB for this small composition; measure actual usage).
No host, domain, billing plan or DNS change is selected automatically by these files.

## Preflight

1. Publish the reviewed repository; confirm anonymous access and CI results.
2. Confirm metadata licensing in DATA-LICENSE.md; name a maintainer and monitored contact.
3. Copy deployment/.env.example to deployment/.env and fill every field. Generate a
   unique password using `openssl rand -hex 32`; the URL construction expects an
   alphanumeric password. Restrict the environment file to mode 600.
4. Export those values in a shell from your trusted secret manager, then run
   `python -m scripts.check_launch`. Do not source untrusted environment files.
5. Point the approved hostname to the host. Allow inbound 80/443; restrict administrative
   SSH separately. Do not expose the database or API ports. Verify provider log retention.

## Start

From the repository root:

```sh
docker compose --env-file deployment/.env -f deployment/compose.yaml config --quiet
docker compose --env-file deployment/.env -f deployment/compose.yaml up --build -d
python -m scripts.smoke_public https://open.alvary.ai
```

Substitute the actual approved hostname. Caddy automatically obtains TLS after DNS
and port reachability are correct. The one-shot migration service must succeed before
API starts. A failed health check blocks the web dependency at initial startup.
Containers restart after process failure; unhealthy state alone does not trigger repair.
An external uptime monitor must alert a named operator on `/api/healthz` failure.

Do not expose releases through an unguarded static bucket or CDN path. nginx forwards
all release requests through the API's integrity/current-rights checks. Avoid caching
API data and downloaded legal content because permissions can be withdrawn.

## Backup and restore exercise

Schedule `deployment/backup.sh` daily using the host's approved scheduler. It creates
a PostgreSQL custom-format dump with restrictive permissions and checks its archive
catalogue. Transfer encrypted copies off-host, retain an agreed window (initial
proposal: 7 daily and 4 weekly), and document the responsible operator. Scheduling,
remote storage and retention are not activated by the script itself.

Before public launch, restore a dump into a **separate test database**, never over
the live database. Use `pg_restore --exit-on-error --no-owner --dbname TEST_DATABASE`
with an explicit temporary destination, then run migrations/seed checks and public
smoke checks against that test deployment. Record backup timestamp and restore result.
Do not run the seed smoke check expecting four sources once the corpus has expanded.

## Upgrade and rollback

Record the deployed commit and image IDs. Take and validate a backup before migration.
Run CI and clean-container checks for the proposed revision. Deploy immutable reviewed
revisions; do not edit code inside running containers. Verify `/api/healthz`, coverage,
search and download hashes. If a code-only regression occurs, redeploy the previous
known-good revision when schema-compatible. Never blindly downgrade a live schema.
For destructive migration failure, stop writes and plan restoration from the verified
backup with the operator; this requires an explicit data-loss assessment.

## Rights complaint or incident

Do not publish private evidence. The maintainer first records the affected source/version
and severity in a private operations channel. Suspend public visibility or exact-hash
approval through a controlled, validated rights update. API projections take effect on
new requests; old bundles become unavailable. Verify source, search, citation and release
routes all deny the affected data. Do not delete historical evidence to hide an error.
If the scope is uncertain, stop public API distribution while reviewing. Previously
downloaded copies cannot be recalled. Publish a suitable correction/withdrawal notice.

## Local clean-container verification

`deployment/smoke.yaml` exposes only the web service on 127.0.0.1:5179. Use a disposable
environment file and project name; start only `web` and its dependencies so Caddy does
not request a real certificate. This validates the production images and internal
routing, not live DNS/TLS or an actual hosting provider.
