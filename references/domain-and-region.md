# Domain and Region Guide

Use this reference when the user does not own a domain, asks about DNS, needs Route 53, is located in mainland China, or needs to choose a nearby AWS Region.

## Default Assumption for Mainland China Users

Assume a beginner in mainland China should start with an overseas domain registrar and a non-mainland AWS Region unless they explicitly need mainland China hosting, ICP filing, local telecom compliance, or China-specific cloud operations.

Reasons:

- Mainland China domain, hosting, ICP filing, real-name verification, content rules, and cloud-provider workflows add operational and legal complexity.
- AWS China Regions are operated separately by local partners and require separate China-specific account credentials.
- If the project is an early personal, learning, portfolio, SaaS prototype, or overseas-facing service, a global AWS account plus a nearby Region is usually simpler.

Suggested starting Regions to evaluate:

- `ap-northeast-2` - Asia Pacific (Seoul), physically close to northern/eastern China and often a good first test.
- `ap-southeast-1` - Asia Pacific (Singapore), commonly used for Southeast Asia and many China-adjacent products.
- Consider `ap-east-1` Hong Kong only if service availability, cost, and account opt-in fit the project.

Always test latency from the user's actual network before committing to production. Do not promise China mainland network performance; international connectivity can vary by carrier, time, and content.

## Domain Ownership Decision

If the user has no domain, guide them through this decision before production HTTPS work:

1. Choose a name and TLD such as `.com`, `.net`, `.dev`, or another globally supported TLD.
2. Prefer a registrar that supports easy authoritative nameserver changes.
3. If the domain can be registered directly in Route 53 Domains, recommend that first because registration and DNS can stay in AWS.
4. If Route 53 registration is unavailable or inconvenient, suggest an overseas registrar that clearly lets users edit custom nameservers, then delegate DNS to Route 53.
5. Avoid registrars or plans that lock the domain to the registrar's DNS if the goal is to manage DNS in Route 53.

Good registrar criteria:

- Supports the desired TLD and the user's payment method.
- Allows custom nameserver changes without support tickets.
- Provides WHOIS privacy where supported.
- Has account MFA.
- Has clear renewal pricing and renewal controls.
- Does not require mainland China real-name or ICP workflow for a simple overseas-hosted beginner project.

Examples to evaluate, not endorsements:

- Route 53 Domains: simplest if the chosen TLD and account can register the domain directly.
- Porkbun: has a nameserver edit flow and is commonly used for inexpensive global domains.
- Namecheap: has custom DNS/nameserver settings and broad TLD support.

Cloudflare can be a strong DNS/CDN provider, but if the explicit goal is "Route 53 manages authoritative DNS", verify before buying that the registrar path allows third-party nameserver delegation to Route 53.

## Route 53 Delegation Flow

Teach these concepts as part of the flow:

- Registrar: the company where the user buys or renews the domain.
- Registry: the organization that runs a TLD such as `.com`.
- DNS hosted zone: the Route 53 container for records for one domain.
- Nameserver delegation: telling the registrar that Route 53's four nameservers are authoritative for the domain.
- DNS records: instructions such as `A`, `AAAA`, `CNAME`, `MX`, and `TXT`.

Recommended sequence:

1. Ask whether the user already owns a domain.
2. If not, help them choose 2-3 candidate domains and a registrar path.
3. Explain costs: annual domain registration, Route 53 hosted zone monthly charge, DNS query charges, and server charges.
4. Have the user buy the domain themselves in the registrar UI. Do not ask for registrar password, card, or verification code in chat.
5. Create a Route 53 public hosted zone for the domain.
6. Copy the four Route 53 NS values from the hosted zone.
7. In the registrar UI, replace the domain's authoritative nameservers with the four Route 53 nameservers.
8. Wait for delegation propagation. Explain that this can take minutes to 48 hours depending on caches and registry/registrar behavior.
9. Verify delegation before deployment:

```powershell
nslookup -type=NS <domain>
```

10. Add initial records only after the user confirms the target:
    - `A` record to an EC2 Elastic IP for simple server deployment.
    - `CNAME` record for subdomains pointing to another hostname.
    - `MX` and `TXT` only if email is configured.

## Mainland China Caveats

Do not present this as legal advice. Explain that mainland China hosting can require ICP filing and related real-name or public security workflows, and that the exact requirements depend on the hosting location, cloud provider, domain, business type, and content.

Practical beginner guidance:

- If the service can be hosted outside mainland China, start outside mainland China to reduce setup complexity.
- If the service must be fast and compliant for mainland China users, pause and create a separate China deployment plan before buying infrastructure.
- If using AWS China Regions, explain that they are separate from ordinary global AWS accounts and use separate credentials and local operating partners.
- Avoid `.cn` or China-specific TLDs for a first project unless the user explicitly needs them and understands the identity/compliance workflow.

## Documentation

When a domain or DNS path is chosen, record non-secret facts in `docs/deploy.md`:

- Registrar name.
- Domain name.
- Whether Route 53 registered the domain or only manages DNS.
- Route 53 hosted zone ID.
- AWS account/profile and Region.
- Current nameserver delegation status.
- Which DNS records belong to the project.

Never record registrar passwords, payment details, account recovery codes, contact verification links, or private account IDs in docs.

## Official Sources

- Route 53 domain registration and transfer: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/registrar.html
- Route 53 hosted zones and nameserver delegation: https://docs.aws.amazon.com/cli/latest/reference/route53/create-hosted-zone.html
- AWS Regions list: https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions.html
- AWS China gateway and separate account requirements: https://aws.amazon.com/china-gateway
- ICP filing overview for mainland China hosting: https://www.alibabacloud.com/help/doc-detail/102064.html
- Porkbun nameserver change help: https://kb.porkbun.com/article/22-how-to-change-nameservers
- Namecheap custom DNS help: https://www.namecheap.com/support/knowledgebase/article.aspx/767/10/how-to-change-dns-for-a-domain
