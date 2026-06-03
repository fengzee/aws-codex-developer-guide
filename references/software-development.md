# Software Development Workflow

Use this reference for GitHub, branches, auto-commit, CI, documentation, Codebase-Memory MCP, and ongoing Codex collaboration practices.

## Repository Baseline

Every project should have:

- A git repository with a clean default branch.
- A root `AGENTS.md` that defines context loading, hard rules, verification commands, and documentation expectations.
- Directory-level `AGENTS.md` files for areas with local constraints, such as infrastructure, deployment, backend, frontend, scripts, data, or docs.
- `docs/architecture.md`, `docs/deploy.md`, `docs/operations.md`, and `docs/worklog.md` when the project has cloud resources or persistent operational state.
- `.env.example` and infrastructure variable examples with non-secret placeholder values.
- A single verification entry point such as `make verify`, `npm test`, or `just check`.

## Branch Management

- Keep `main` or `master` deployable.
- Use short-lived feature branches, preferably `codex/<task-slug>` for Codex-driven work.
- Commit after a coherent, verified change. Do not leave completed work as an unexplained dirty worktree.
- Push branches and open pull requests for risky, public, or collaborative changes.
- Delete merged local and remote feature branches only after confirming they are merged and not owned by an active thread or PR.
- Avoid force-pushing shared branches. If force-push is needed for a personal feature branch, say why.

For important branches, configure GitHub branch protection or rulesets: require status checks, disallow deletion, and require PR review when collaboration or production risk justifies it.

## GitHub Auto-Commit

There are two different meanings. Keep them separate.

### Codex local auto-commit

Use `AGENTS.md` to tell Codex when to commit:

```markdown
After code, config, script, or documentation changes pass verification, commit them on the current `codex/` branch unless the user explicitly asks not to commit. If verification fails or work is incomplete, leave the worktree dirty and explain why.
```

This is appropriate for interactive development where Codex is operating the user's local repository.

### GitHub Actions generated commits

Use a workflow only for deterministic generated files: dependency locks, static reports, generated docs, data snapshots, or scheduled updates. Avoid workflows that run open-ended AI editing on the default branch.

Rules:

- Give the workflow `contents: write` only when it must push commits.
- Commit only when `git diff` shows changes.
- Use a bot identity.
- Prefer a dedicated automation branch or pull request when `main` is protected.
- Keep secrets in GitHub Actions secrets, never in workflow files.
- Make the generator command deterministic and idempotent.

Use `assets/github-workflows/auto-commit.yml.template` as a starting point.

## Documentation Layers

Use documentation as the durable memory of the project.

| File | Purpose |
| --- | --- |
| `AGENTS.md` | Project-level entry point, hard rules, context order, verification |
| `*/AGENTS.md` | Local rules and boundaries for a directory |
| `README.md` | Human entry point and quick start |
| `docs/architecture.md` | Long-term system design, data model, auth model, service boundaries |
| `docs/deploy.md` | Deployment flow, environments, AWS resources, rollback |
| `docs/operations.md` | Routine maintenance, backups, monitoring, incident steps |
| `docs/worklog.md` | Executed work, verification results, temporary observations |
| `.env.example` | Non-secret configuration contract |

Do not duplicate long rules across many files. Put the canonical version in the most specific stable document and point to it from indexes.

## Documentation Update Matrix

| Change | Must update |
| --- | --- |
| New API, page, or feature | README or feature docs, tests |
| New persistent data model | architecture docs, migration notes, tests |
| New environment variable | `.env.example`, config docs |
| New deployment, backup, or AWS resource | deploy docs, operations docs, `AGENTS.md` if it creates a rule |
| New invariant every future agent must obey | nearest `AGENTS.md`, root `AGENTS.md` if project-wide |
| Completed operational work | `docs/worklog.md` |

## Codebase-Memory MCP

Use Codebase-Memory MCP when a repository is large enough that repeated grep/file search wastes context or misses call relationships.

Recommended `AGENTS.md` snippet:

```markdown
## Codebase-Memory MCP

Prefer Codebase-Memory MCP for code discovery:

1. `search_graph` for functions, classes, routes, and variables.
2. `trace_path` for caller/callee and impact analysis when available.
3. `get_code_snippet` after `search_graph` identifies an exact symbol.
4. `query_graph` for complex structural questions.
5. `get_architecture` for high-level orientation.

Fall back to `rg` or file search for string literals, error messages, config values, non-code files, or insufficient MCP results. Always verify source before editing.
```

Indexing and persistence are project decisions. If sharing graph artifacts in git, document which files are committed, how to refresh them, and how large the artifacts are expected to become.

## Verification and Hooks

Provide one high-signal command:

```bash
make verify
```

It should run the project's equivalent of:

- Formatting or lint checks.
- Unit and integration tests.
- Infrastructure or deployment guardrails.
- Documentation or generated-file checks.

Optional hooks:

- `pre-commit`: fast guardrails and format checks.
- `pre-push`: full verification.

Do not let hooks become the only documented validation path. Codex needs an explicit command it can run and report.

## Official Sources

- GitHub Actions workflows: https://docs.github.com/actions/using-workflows/about-workflows
- GitHub Actions workflow syntax: https://docs.github.com/actions/reference/workflows-and-actions/workflow-syntax
- GitHub protected branches: https://docs.github.com/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches/
- GitHub CLI overview: https://docs.github.com/get-started/using-github/github-cli
