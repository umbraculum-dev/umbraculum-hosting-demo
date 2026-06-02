# umbraculum-hosting-demo

Operator repo for **`demo.umbraculum.dev`** — Traefik TLS edge + production Docker Compose (planned).

**Start here** for the demo VPS. Forum ops: [umbraculum-hosting-forum](https://github.com/umbraculum-dev/umbraculum-hosting-forum).

## Status

Scaffold only. Full stack per [demo-vps-bootstrap plan](https://github.com/umbraculum-dev/umbraculum-dev) and `docs/OPERATOR.md` (compose + Traefik land here in a follow-up PR).

## Quick start (VPS, when compose exists)

```bash
git clone --recurse-submodules git@github.com:umbraculum-dev/umbraculum-hosting-demo.git /opt/umbraculum-hosting-demo
git clone git@github.com:umbraculum-dev/umbraculum-dev.git /opt/umbraculum-dev
cd /opt/umbraculum-hosting-demo
bin/harden    # as root
bin/pull
# docker compose -f docker-compose.demo.yml up -d  (when added)
```

## Submodule

| Path | Repo |
|------|------|
| `common/` | [umbraculum-hosting-common](https://github.com/umbraculum-dev/umbraculum-hosting-common) |

Use **`bin/pull`** on every update.

## Product docs (umbraculum-dev)

- [demo-host-runbook.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-runbook.md)
- [production-hosts.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/production-hosts.md)
- Verify: `scripts/demo-host-verify.sh` in umbraculum-dev clone on demo VPS

## Local laptop layout

`/home/rf/dkprojects/rfapps/umbraculum-hosting/umbraculum-hosting-demo/` alongside forum and common clones.
