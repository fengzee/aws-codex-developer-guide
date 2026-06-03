# AWS Environment Guide

Use this reference when a task touches AWS account setup, IAM, CLI access, EC2, SSM, Docker, Caddy, Route 53, S3, Terraform, backups, deployment, or production verification.

## Baseline Principles

- The user's AWS account is their environment. Do not reuse private example resource names, domains, public IPs, hosted zones, buckets, instance IDs, or account IDs.
- Assume the user's local computer is Windows unless they explicitly say otherwise. Use Windows/PowerShell-friendly local commands by default; use Linux shell syntax only for remote EC2 commands or when the user says they are on macOS/Linux.
- Secure the root user first: strong password, MFA or passkey/security key where available, no root access keys, and minimal root-user use.
- Prefer temporary credentials for humans and workloads. Use IAM Identity Center or role assumption where practical.
- A dedicated IAM user with long-lived access keys can be acceptable for local Codex bootstrap only when the user understands the tradeoff. Keep it local, name it clearly, grant only the permissions needed as the project matures, rotate keys, and remove it if a role or Identity Center flow replaces it.
- Use a named AWS CLI profile for Codex operations. Never paste the access key ID or secret access key into chat, docs, scripts, GitHub secrets, or repository files unless the task is explicitly to create a secure secret store entry.
- Prefer SSM Session Manager or SSM Run Command over SSH. A well-configured EC2 should not need public SSH ingress for routine Codex operations.

## Account Bootstrap

1. Confirm the user's target AWS account and region.
   - For a mainland China beginner without a mainland compliance requirement, suggest starting with a global AWS account in a nearby non-mainland Region such as Seoul (`ap-northeast-2`) or Singapore (`ap-southeast-1`), then test latency.
   - Load `references/domain-and-region.md` if the user has no domain, mentions China, or needs DNS/Route 53 decisions.
2. Secure root access and confirm MFA status.
3. Create a human admin path:
   - Preferred: IAM Identity Center user or federated identity with administrator permission set during bootstrap.
   - Simpler local automation path: a dedicated IAM user such as `codex-admin` with `AdministratorAccess` only for bootstrap, then reduce permissions.
4. Configure the AWS CLI profile locally:

```powershell
aws configure --profile <profile-name>
aws --profile <profile-name> --region <region> sts get-caller-identity
```

5. Record only non-secret values in docs: profile name, default region, account alias, resource naming convention, and how to verify identity.

## Infrastructure Baseline

For a small personal or early-stage project, a simple baseline is often enough:

- One AWS region chosen deliberately for user proximity, data residency, and cost.
- For mainland China users, avoid AWS China Regions unless they explicitly need mainland hosting/compliance and understand the separate account and filing workflow.
- Terraform as the source of truth for IAM, VPC/security group, EC2, EIP if needed, Route 53, S3, and lifecycle policies.
- One EC2 host for small workloads, with an instance role that allows SSM management and only required AWS API calls.
- Docker Compose per project, with explicit project names, service names, named volumes, and log rotation.
- Caddy or another reverse proxy as the HTTPS entrypoint, with marked config blocks for each project/domain.
- Route 53 records scoped to the project domain or subdomain.
- S3 prefixes scoped by project for release bundles, backups, and temporary artifacts.
- Backups and cleanup scripts that dry-run by default and match exact project prefixes or labels.

Use separate accounts, environments, or hosts when projects have different owners, compliance needs, data sensitivity, availability requirements, or cost centers.

## Shared Host Pattern

When multiple apps share one EC2:

- Document ownership of each domain, container, volume, host path, S3 prefix, port, and Caddy block.
- Keep reverse-proxy blocks clearly marked and preserve neighbor blocks during edits.
- Attach services to an explicit external Docker network only when cross-compose routing is needed.
- Run only scoped Docker commands:

```powershell
docker compose -f <compose-file> --project-name <project> ps
docker compose -f <compose-file> --project-name <project> up -d <service>
docker compose -f <compose-file> --project-name <project> logs --tail=200 <service>
```

- Avoid global commands such as `docker system prune`, unscoped `docker compose down`, broad `docker stop`, and production `--remove-orphans`.
- Verify the changed service and any neighbor services whose proxy, network, port, or security group could have been affected.

## Terraform and Route 53

Before applying infrastructure changes:

```powershell
terraform fmt -check
terraform validate
terraform plan
```

Treat these plan signals as blockers until understood:

- EC2 replacement, EIP reassociation, or volume deletion.
- Security group rules removed or widened outside the intended service.
- Route 53 changes outside the intended zone or hostname.
- S3 lifecycle prefixes broadened from project-specific paths to bucket-wide paths.
- IAM policy broadening without a named deployment, backup, or operations need.

For Route 53, document the hosted zone, record names, record type, target, TTL, and which service owns the DNS name. Keep apex and wildcard records deliberate.

## SSM Deployment

Prefer SSM because it avoids long-lived SSH credentials and unnecessary inbound ports.

Minimum flow:

1. Confirm identity:

```powershell
aws --profile <profile-name> --region <region> sts get-caller-identity
```

2. Resolve the instance ID from Terraform output, tags, or explicit config.
3. Upload a release bundle to a project-owned S3 prefix or render files locally for SSM transfer.
4. Use `AWS-RunShellScript` or an SSM session to run idempotent commands in the target project directory.
5. Capture command ID, stdout, stderr, and status on failure.
6. Verify HTTPS endpoints, Docker service status, recent logs, and backup state when relevant.

## Delivery Checklist

Before live AWS work:

- Confirm profile, region, account, target project, and billable resources.
- Read the repository `AGENTS.md` and deployment docs.
- Inspect current Terraform plan, Caddy blocks, Compose services, Docker networks, S3 prefixes, and backup scripts.
- Identify rollback: revert commit, restore previous Caddyfile, redeploy previous image/bundle, restore snapshot, or undo DNS.

After live AWS work:

- Run health checks for changed endpoints.
- Inspect service logs and `docker compose ps`.
- Verify DNS and TLS if Route 53 or proxy changed.
- Verify backup object creation or restore instructions if backup behavior changed.
- Update docs and commit verified changes.

## Official Sources

- AWS IAM best practices: https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html
- AWS root user best practices: https://docs.aws.amazon.com/IAM/latest/UserGuide/root-user-best-practices.html
- AWS CLI named profiles: https://docs.aws.amazon.com/cli/latest/reference/configure/
- AWS Systems Manager Session Manager: https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html
