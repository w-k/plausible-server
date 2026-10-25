# plausible-server

Docker Compose deployment of [Plausible CE](https://github.com/plausible/community-edition)
serving analytics.rdnl.dev on the Hetzner server (`/root/plausible`).

Based on `plausible/community-edition` (MIT, see LICENSE). Secrets live only in
`/root/plausible/.env` on the server and are never committed.

## Updates

1. Dependabot checks daily for new image versions and opens a pull request.
2. Review the release notes linked in the PR and merge it to approve.
3. Within 5 minutes the server's `update-plausible.timer` sees the new commit,
   dumps the Postgres database to `/root/backups/`, pulls the images and
   restarts the stack. A push notification reports success or failure.

Major Postgres/ClickHouse versions are ignored by Dependabot because they need a
data migration; follow the community-edition release notes for those.

## Exposure

Caddy (not in this repo, `/etc/caddy/Caddyfile`) terminates TLS. Only `/js/*`
and `/api/event` are public; the dashboard requires basic auth. Plausible itself
listens on `127.0.0.1:8000` only.
