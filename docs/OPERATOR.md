# Demo VPS operator guide (`demo.umbraculum.dev`)

**Repo:** umbraculum-hosting-demo. **Product gates:** [demo-host-runbook.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-runbook.md). **SSL ADR:** [demo-host-ssl-strategy.md](https://github.com/umbraculum-dev/umbraculum-dev/blob/master/docs/design/demo-host-ssl-strategy.md).

---

## Layout on demo VPS

| Path | Repo |
|------|------|
| `/opt/umbraculum-hosting-demo` | This repo — Traefik, `docker-compose.demo.yml`, `.env` |
| `/opt/umbraculum-dev` | Application source — builds, migrations, verify scripts |

---

## Phase 0 — First boot (host only)

As **root** (SSH keys recommended before `bin/harden --ssh-hardening`):

```bash
apt-get update && apt-get install -y git
git clone --recurse-submodules https://github.com/umbraculum-dev/umbraculum-hosting-demo.git /opt/umbraculum-hosting-demo
cd /opt/umbraculum-hosting-demo
bin/bootstrap
```

DNS: `demo.umbraculum.dev` **A** → VPS IPv4 (grey cloud). Verify: `dig +short demo.umbraculum.dev`.

---

## Phase C — Application deploy

### C1 — Update operator repo

```bash
cd /opt/umbraculum-hosting-demo
bin/pull
```

Requires `docker-compose.demo.yml`, `nginx/demo.conf`, and `.env.demo.example` on `main`.

### C2 — Clone application source

```bash
git clone https://github.com/umbraculum-dev/umbraculum-dev.git /opt/umbraculum-dev
cd /opt/umbraculum-dev
# pin a release tag or branch as needed, e.g. git checkout main && git pull
```

### C3 — Build workspace packages (required before compose up)

From `/opt/umbraculum-dev` on the VPS:

```bash
./scripts/build-packages-in-docker.sh --all --fresh
```

Expect several minutes on VPS 10; adds swap if OOM during build (see demo-vps-bootstrap plan).

### C4 — Configure secrets

```bash
cd /opt/umbraculum-hosting-demo
cp .env.demo.example .env
chmod 600 .env
```

Edit `.env`:

- `ACME_EMAIL` — Let's Encrypt contact
- `POSTGRES_PASSWORD` — strong random; keep `DATABASE_URL` in sync
- `APP_AI_KEY_SECRET` — `openssl rand -hex 32` (required for `NODE_ENV=production`)
- `RENDERING_SIGNING_SECRET` — `openssl rand -hex 32` (recommended)

### C5 — Start stack

**Requires** C3 (`build-packages-in-docker.sh --all --fresh`) so Docker volume `umbraculum_root_node_modules` exists. Compose mounts that volume — do not rely on per-app `npm install` alone.

```bash
cd /opt/umbraculum-hosting-demo
docker compose -f docker-compose.demo.yml --env-file .env up -d
```

First start runs production **build** for **api** and **web** workspaces (10–30+ minutes). Tail logs:

```bash
docker compose -f docker-compose.demo.yml logs -f api web traefik
```

Traefik obtains the LE certificate once port 80 is reachable for `demo.umbraculum.dev`.

### C6 — Database migrate + seed

When **api** is healthy:

```bash
cd /opt/umbraculum-hosting-demo
docker compose -f docker-compose.demo.yml --env-file .env exec api \
  sh -c 'npx prisma migrate deploy --schema=/repo/services/api/prisma/schema.prisma'
docker compose -f docker-compose.demo.yml --env-file .env exec api \
  npm run seed:e2e -w @umbraculum/api
```

### C7 — Verify (laptop or VPS)

```bash
/opt/umbraculum-dev/scripts/demo-host-verify.sh
BASE_URL=https://demo.umbraculum.dev /opt/umbraculum-dev/scripts/demo-native-api-smoke.sh
```

Browser: `https://demo.umbraculum.dev/en` — E2E admin per demo-host-runbook.

---

## Maintenance

```bash
cd /opt/umbraculum-hosting-demo
bin/pull
cd /opt/umbraculum-dev && git pull
./scripts/build-packages-in-docker.sh --from-diff HEAD~1 --include-dependents
docker compose -f docker-compose.demo.yml --env-file .env up -d --build
```

`bin/harden` / `bin/bootstrap` — see [hosting-common README](https://github.com/umbraculum-dev/umbraculum-hosting-common/blob/main/README.md).

---

## Isolation

Do **not** install the forum Discourse stack on the demo VPS.
