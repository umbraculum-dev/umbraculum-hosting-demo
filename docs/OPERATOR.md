# Demo VPS operator guide (`demo.umbraculum.dev`)

**Status:** Scaffold. Implementation tracked in umbraculum-dev **demo-vps-bootstrap** plan.

---

## First boot — install git, clone this repo, bootstrap

Same chicken-and-egg as forum: install **git** with `apt`, then clone, then **`bin/bootstrap`**.

As **root**:

```bash
apt-get update && apt-get install -y git
git clone --recurse-submodules https://github.com/umbraculum-dev/umbraculum-hosting-demo.git /opt/umbraculum-hosting-demo
cd /opt/umbraculum-hosting-demo
bin/bootstrap
```

Also clone umbraculum-dev on the demo VPS when the stack is implemented (see below).

---

## Planned layout on demo VPS

| Path | Repo |
|------|------|
| `/opt/umbraculum-hosting-demo` | This repo — Traefik, `docker-compose.demo.yml`, `.env.demo.example` |
| `/opt/umbraculum-dev` | Application source — `npm run build`, packages, `scripts/demo-host-verify.sh` |

## Steps (when implemented)

1. **Provision** Contabo VPS 10, Ubuntu 24.04 (no Auto Backup at bootstrap).
2. First boot block above (`apt` + git + clone + `bin/bootstrap`).
3. Clone umbraculum-dev for image build context.
4. DNS: `demo` A record → VPS (grey cloud).
5. `docker compose -f docker-compose.demo.yml build && up -d` from this repo.
6. Migrate/seed API per umbraculum-dev runbook.
7. Run `demo-host-verify.sh` and native API smoke from umbraculum-dev.

## Hardening / bootstrap (available now)

```bash
cd /opt/umbraculum-hosting-demo
bin/pull
bin/bootstrap    # prereqs + security — fresh or after common submodule bump
bin/harden       # security only
```

## Product / EAS docs

- [demo-host-runbook.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-runbook.md)
- [native-eas-demo-build-log.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/native-eas-demo-build-log.md)

## Isolation

Do **not** install the forum Discourse stack on the demo VPS.
