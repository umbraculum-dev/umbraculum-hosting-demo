# umbraculum-hosting-demo

Operator repo for **`demo.umbraculum.dev`** — Traefik TLS edge + production Docker Compose (planned).

**Start here** for the demo VPS. Forum ops: [umbraculum-hosting-forum](https://github.com/umbraculum-dev/umbraculum-hosting-forum).

## Status

**Phase C:** `docker-compose.demo.yml` + Traefik + `nginx/demo.conf` + `.env.demo.example`. Deploy steps: `docs/OPERATOR.md`.

## Quick start (fresh VPS)

As **root**:

```bash
apt-get update && apt-get install -y git
git clone --recurse-submodules https://github.com/umbraculum-dev/umbraculum-hosting-demo.git /opt/umbraculum-hosting-demo
cd /opt/umbraculum-hosting-demo
bin/bootstrap
```

Then Phase C in `docs/OPERATOR.md` (clone umbraculum-dev, build packages, `.env`, `docker compose -f docker-compose.demo.yml up -d`).

## Submodule

| Path | Repo |
|------|------|
| `common/` | [umbraculum-hosting-common](https://github.com/umbraculum-dev/umbraculum-hosting-common) |

Use **`bin/pull`** on every update. Planned app redeploy: **`bin/redeploy`** (see `docs/OPERATOR.md`).

## Product docs (umbraculum-dev)

- [demo-host-runbook.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-runbook.md)
- [production-hosts.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/production-hosts.md)
- Verify: `scripts/demo-host-verify.sh` in umbraculum-dev clone on demo VPS

## Local laptop layout

`/home/rf/dkprojects/rfapps/umbraculum-hosting/umbraculum-hosting-demo/` alongside forum and common clones.
