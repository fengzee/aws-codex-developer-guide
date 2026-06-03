---
name: aws-codex-developer-guide
description: Guide first-time software developers through AWS-backed, Codex-assisted software projects while teaching cloud and software engineering concepts through practice. Use when Codex needs to bootstrap or audit an AWS account for development, configure AWS CLI or Codex AWS access, assume a Windows local environment unless the user says otherwise, explain AWS accounts, IAM, EC2, Docker, DNS, Route 53, S3, Terraform, SSM, Git, branches, tests, CI, documentation, or Codebase-Memory MCP to beginners, adjust explanation depth to the user's demonstrated understanding, or plan and operate AWS-backed development workflows.
---

# AWS Codex Developer Guide

## Purpose

Use this skill to help a non-expert user establish their own AWS-backed development environment and then keep building software with Codex in a controlled, documented way.

This skill is a guide and template bundle, not a cloud resource inventory. Never copy private resource names, IP addresses, domains, bucket names, account IDs, or credentials from an example environment into a new user's project.

## Operating Model

1. Translate the user's natural-language request into a small phase: account bootstrap, identity setup, infrastructure baseline, deployment, GitHub workflow, documentation, or maintenance.
2. Read the current repository's `AGENTS.md`, nearest directory `AGENTS.md`, and deploy or operations docs before touching existing files.
3. Assume the user's local machine is Windows unless they explicitly say otherwise. Prefer Windows/PowerShell-friendly local commands; use Linux shell commands only for remote Linux hosts or when the user says they are on macOS/Linux.
4. Load only the needed reference:
   - Teaching mode, concept explanation, learner-level adjustment, and learning notes: `references/learning-mode.md`.
   - AWS accounts, IAM, CLI profiles, EC2, Docker, Caddy, Route 53, S3, Terraform, or SSM: `references/aws-environment.md`.
   - GitHub auto-commit, branches, CI, documentation structure, or Codebase-Memory MCP: `references/software-development.md`.
   - Beginner-facing request examples and safe clarification patterns: `references/beginner-prompts.md`.
5. When starting a new project, copy and adapt templates from `assets/project-docs/` and `assets/github-workflows/`.
6. Before live AWS changes, state the account/profile, region, resources to be created or changed, expected cost class, rollback path, and verification commands.
7. After changes, verify behavior, update docs, and commit on the active feature branch unless the user explicitly asks not to commit.

## Guardrails

- Do not ask the user to paste secrets into chat. Keep AWS access keys, `.env` values, Terraform state, deploy bundles, SSH keys, GitHub tokens, and production logs out of git and conversation.
- Do not create or use root-user access keys. Secure the root user first, then use IAM Identity Center, roles, or a dedicated IAM user/profile for local automation.
- If a dedicated local Codex admin profile is used for bootstrap, treat it as a temporary or tightly governed exception: local credentials file only, MFA or compensating controls where practical, regular rotation, and a plan to reduce to least privilege.
- Prefer SSM Session Manager or Run Command over opening SSH. Do not open broad inbound ports unless the user understands the exposure and the service needs it.
- Treat a single EC2 running multiple Docker services as a shared production host: use explicit Compose files, service names, networks, volumes, labels, marked reverse-proxy blocks, and neighbor health checks.
- Keep Route 53 records, S3 prefixes, Terraform resources, Docker cleanup, and backup retention scoped to the project owner. Avoid global cleanup and broad lifecycle rules.
- Use Terraform plans, branch diffs, tests, and health checks as delivery gates. Do not apply infrastructure or deploy production code when identity, scope, or rollback is unclear.

## Beginner Communication

For non-expert users, keep the interaction concrete:

- Teach through the current task. Before each meaningful step, explain the concept in 2-5 sentences, then do or plan the step, then recap what the user just learned.
- For local setup steps, say when instructions are Windows-specific and ask only if the user appears to be on a different OS.
- Explain what will exist after the step, not only which command will run.
- Ask only for blocking inputs: AWS account access, preferred region, domain name, GitHub repository, budget limit, and whether production traffic exists.
- Separate reversible local edits from irreversible or billable cloud actions.
- Give the user a small command or URL to inspect after each phase.
- Use the user's answers, questions, and error descriptions to infer whether they need beginner, working, or advanced explanations.
- Record stable decisions in docs instead of relying on chat history.

## Project Handoff

When establishing a repository for ongoing Codex work, create or update:

- Root `AGENTS.md` as the collaboration entry point and hard-rule index.
- Directory-level `AGENTS.md` files for deployment, infrastructure, backend, frontend, scripts, or data boundaries.
- `docs/architecture.md`, `docs/deploy.md`, `docs/operations.md`, and `docs/worklog.md`.
- `.env.example`, infrastructure variable examples, and secret-handling notes.
- A deterministic verification command such as `pwsh ./scripts/verify.ps1`, `npm test`, `pytest`, or `make verify` that runs formatting, tests, config checks, and documentation guardrails.
- Codebase-Memory MCP instructions when the project is large enough that graph search will reduce repeated file scanning.
- For long beginner onboarding, optional `docs/learning-notes.md` that records learned concepts, open questions, and the current explanation level.

Use the bundled templates as starting points, then replace every placeholder with the user's own account, project, domain, region, and verification commands.
