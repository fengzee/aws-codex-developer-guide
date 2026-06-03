# Operations

## Routine Checks

- Application health: `<command or URL>`
- Docker service status: `<command>`
- Backups: `<command>`
- Disk usage: `<command>`
- TLS/DNS: `<command>`

## Backups

- What is backed up: `<data>`
- Where backups are stored: `<non-secret S3 bucket/prefix or system>`
- Schedule: `<schedule>`
- Restore command or runbook: `<steps>`

## Monitoring

- Logs: `<where>`
- Alerts: `<where>`
- Dashboards: `<where>`

## Incident Steps

1. Confirm scope and affected users.
2. Preserve logs and current state.
3. Check recent commits, deploys, AWS changes, DNS, disk, and service status.
4. Roll back or apply a minimal fix.
5. Record findings in `docs/worklog.md`.

## Cleanup Rules

- Dry-run first.
- Match exact project-owned paths, labels, service names, or S3 prefixes.
- Do not run global Docker cleanup or bucket-wide deletion from this project.
