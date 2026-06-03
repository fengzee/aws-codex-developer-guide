# Beginner Prompt Patterns

Use this reference when helping a user who does not know what to ask for.

When the user wants to learn while building, also load `references/learning-mode.md`.
Assume the user's local machine is Windows unless they explicitly say otherwise.

## AWS Setup Prompts

```text
Use $aws-codex-developer-guide to help me set up a new AWS account for a small web app. I use Windows unless I say otherwise. I own the account, I want the region to be <region>, and my budget ceiling is <amount>. Explain each billable or security-sensitive step before doing it, and teach me the related AWS concepts as we go.
```

```text
Use $aws-codex-developer-guide to configure Codex AWS CLI access on this Windows machine. I want a named profile, no secrets in chat or git, and a verification command that proves the profile works. Explain what AWS accounts, IAM, profiles, and permissions mean in beginner language.
```

```text
Use $aws-codex-developer-guide to plan a simple EC2 + Docker + Caddy + Route 53 deployment for <domain>. Create docs and Terraform placeholders, but do not apply live AWS changes until I confirm.
```

## Project Setup Prompts

```text
Use $aws-codex-developer-guide to turn this folder into a reliable Codex-operated project. Add AGENTS.md, docs, .env.example, verification commands, and branch/commit rules.
```

```text
Use $aws-codex-developer-guide to help me learn software engineering by building this project. Explain Git, branches, commits, tests, CI, documentation, and deployment concepts only when they become relevant to the task.
```

```text
Use $aws-codex-developer-guide to add Codebase-Memory MCP instructions to this repository. Prefer graph search for code discovery, but keep rg as a fallback for configs and strings.
```

```text
Use $aws-codex-developer-guide to create a GitHub Actions workflow that commits deterministic generated files. Use a safe bot identity and only commit when files actually changed.
```

## Deployment and Maintenance Prompts

```text
Use $aws-codex-developer-guide to review whether this deployment change is safe for my AWS resources. Check Terraform, Docker, Caddy, Route 53, S3, secrets, rollback, and health checks.
```

```text
Use $aws-codex-developer-guide to document my current production resources without recording secrets. I want future Codex sessions to know what each resource is for and how to verify it.
```

```text
Use $aws-codex-developer-guide to clean up unused AWS resources. Start with an inventory and a dry-run plan. Do not delete anything until I explicitly approve the exact list.
```

## Safe Clarifying Questions

Ask only what blocks safe progress:

- Which AWS account and region should be used?
- Do you already own the domain or should we defer DNS?
- Is this production traffic or a new test environment?
- What monthly budget range should the plan stay within?
- Should Codex commit verified changes automatically on feature branches?
- Should live AWS changes stop at a plan until you approve?

When the user is unsure, choose a reversible local step first: create docs, inspect configuration, run a plan, or produce a dry-run inventory.
