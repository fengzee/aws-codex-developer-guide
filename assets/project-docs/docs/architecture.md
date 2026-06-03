# Architecture

## Purpose

`<project-name>` exists to `<user/problem/outcome>`.

## System Overview

- Frontend: `<framework or none>`
- Backend: `<framework or none>`
- Database/storage: `<database/storage>`
- Background jobs: `<queue/scheduler/none>`
- External services: `<AWS/GitHub/other>`

## Runtime Boundaries

- Local development: `<commands and ports>`
- Production: `<AWS account, region, host/container/service model>`
- Domains: `<domain names or none>`
- Authentication: `<auth model>`

## Data Model

Document durable facts, derived data, retention, and backup expectations.

## Security Model

- Secrets live in `<secret store/local env/GitHub Actions secrets>`.
- Public endpoints: `<list>`
- Admin-only endpoints: `<list>`
- AWS access path: `<profile/role/SSM>`

## Code Map

| Area | Path | Notes |
| --- | --- | --- |
| App entry | `<path>` | `<notes>` |
| Tests | `<path>` | `<notes>` |
| Deploy | `<path>` | `<notes>` |

## Invariants

- `<rule future Codex sessions must preserve>`
