# AGENTS.md — umbraculum-hosting-demo

VPS operations for **`demo.umbraculum.dev`** only.

## Scope

- Traefik + production compose artifacts belong **in this repo** (not umbraculum-dev root `docker-compose.yml`).
- App **build source** remains [umbraculum-dev](https://github.com/umbraculum-dev/umbraculum-dev) on the demo VPS (e.g. `/opt/umbraculum-dev`).
- Shared hardening: submodule `common/` + `bin/harden`.
- **Do not** add Discourse / forum steps here.

## Submodule

Use `bin/pull`. When changing `common/`, follow [umbraculum-hosting-common SYNC.md](https://github.com/umbraculum-dev/umbraculum-hosting-common/blob/main/SYNC.md).

## Gates

ci-parity / API integration tests apply to **umbraculum-dev** changes, not this repo until compose is added. No secrets in git.

## Operator doc

[`docs/OPERATOR.md`](docs/OPERATOR.md)
