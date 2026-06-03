# Project Agent Notes

This file is the entry point for Codex and other automation collaborators. Keep it short, specific, and current.

## Context Order

1. Read this file first.
2. Read the nearest directory-level `AGENTS.md` before editing files in a subdirectory.
3. Read task-relevant docs:
   - Architecture and data model: `docs/architecture.md`
   - Deployment and cloud resources: `docs/deploy.md`
   - Operations, backups, monitoring, incidents: `docs/operations.md`
   - Recent execution history: `docs/worklog.md`
   - Beginner learning progress, if present: `docs/learning-notes.md`

## Project Snapshot

- Project name: `<project-name>`
- Purpose: `<one-sentence-purpose>`
- Primary stack: `<language/framework/database>`
- Production environment: `<none|AWS region/account/profile/domain>`
- Default local OS assumption: Windows/PowerShell unless the owner documents otherwise.
- Default verification command: `<pwsh ./scripts/verify.ps1|npm test|pytest|make verify|...>`

## Codebase-Memory MCP

Prefer Codebase-Memory MCP for code discovery when available:

1. `search_graph` for functions, classes, routes, and variables.
2. `trace_path` for caller/callee impact analysis when available.
3. `get_code_snippet` after `search_graph` identifies an exact symbol.
4. `query_graph` for complex structural questions.
5. `get_architecture` for high-level orientation.

Fall back to `rg` or file search for string literals, error messages, config values, non-code files, or insufficient MCP results. Always verify source before editing.

## Cloud and Secret Boundaries

- AWS profile: `<profile-name>`
- AWS region: `<region>`
- Do not commit `.env`, AWS credentials, Terraform state, deployment bundles, database dumps, private keys, or production logs.
- Before live AWS changes, confirm identity with:

```powershell
aws --profile <profile-name> --region <region> sts get-caller-identity
```

- For production hosts, prefer SSM over SSH and keep Docker, S3, Route 53, and cleanup commands scoped to this project.

## Branch and Commit Rules

- Use `codex/<task-slug>` branches for substantial Codex-driven work.
- Run `<verification-command>` before final delivery.
- After verified code, config, script, or documentation changes, commit on the current branch unless the user explicitly asks not to commit.
- If verification fails or work is incomplete, leave the worktree dirty and explain why.

## Documentation Contract

- New permanent rule: update the nearest `AGENTS.md`.
- New architecture, data, auth, or service boundary: update `docs/architecture.md`.
- New deploy, AWS, backup, or rollback behavior: update `docs/deploy.md` and `docs/operations.md`.
- Completed operational work: append `docs/worklog.md` with commands and results.
- New configuration value: update `.env.example`.
- If the owner is learning through Codex, update `docs/learning-notes.md` when their explanation preference or covered concepts change.
