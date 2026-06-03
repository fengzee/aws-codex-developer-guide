# Deployment

## Environments

| Environment | AWS profile | Region | Domain | Notes |
| --- | --- | --- | --- | --- |
| local | none | none | localhost | `<notes>` |
| production | `<profile>` | `<region>` | `<domain>` | `<notes>` |

## AWS Resources

Record non-secret resource facts only.

| Resource | Owner | Purpose | Verification |
| --- | --- | --- | --- |
| EC2 instance | `<project>` | `<purpose>` | `<command>` |
| Security group | `<project>` | `<ports>` | `<command>` |
| Route 53 record | `<project>` | `<domain>` | `<command>` |
| S3 prefix/bucket | `<project>` | `<deploy/backups>` | `<command>` |

## Preflight

```powershell
<verification-command>
aws --profile <profile> --region <region> sts get-caller-identity
terraform -chdir=<terraform-dir> fmt -check
terraform -chdir=<terraform-dir> validate
terraform -chdir=<terraform-dir> plan
```

## Deploy Steps

1. Build or package the release.
2. Upload artifacts to a project-owned prefix if needed.
3. Use SSM or the documented deploy mechanism to update only this project.
4. Restart only required services.
5. Run health checks.

## Health Checks

```powershell
curl.exe -I https://<domain>/healthz
docker compose -f <compose-file> --project-name <project> ps
docker compose -f <compose-file> --project-name <project> logs --tail=200 <service>
```

## Rollback

Document how to restore the previous release, reverse DNS/proxy changes, and recover data.
