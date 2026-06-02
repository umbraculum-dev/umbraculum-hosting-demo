# Demo VPS operator guide (`demo.umbraculum.dev`)

**Status:** Scaffold. Implementation tracked in umbraculum-dev **demo-vps-bootstrap** plan.

---

## Planned layout on demo VPS

| Path | Repo |
|------|------|
| `/opt/umbraculum-hosting-demo` | This repo — Traefik, `docker-compose.demo.yml`, `.env.demo.example` |
| `/opt/umbraculum-dev` | Application source — `npm run build`, packages, `scripts/demo-host-verify.sh` |

## Steps (when implemented)

1. **Provision** Contabo VPS 10, Ubuntu 24.04 (no Auto Backup at bootstrap).
2. Clone this repo with submodules; `bin/harden` as root.
3. Clone umbraculum-dev for image build context.
4. DNS: `demo` A record → VPS (grey cloud).
5. `docker compose -f docker-compose.demo.yml build && up -d` from this repo.
6. Migrate/seed API per umbraculum-dev runbook.
7. Run `demo-host-verify.sh` and native API smoke from umbraculum-dev.

## Hardening (available now)

```bash
cd /opt/umbraculum-hosting-demo
bin/pull
bin/harden
```

## Product / EAS docs

- [demo-host-runbook.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-runbook.md)
- [native-eas-demo-build-log.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/native-eas-demo-build-log.md)

## Isolation

Do **not** install the forum Discourse stack on the demo VPS.
