# Learning Mode

Use this reference whenever the user is a beginner, asks "what is this", or wants to learn AWS, servers, Docker, GitHub, or software engineering through the setup process.

## Teaching Goal

Help the user become operationally capable, not just finish the setup. The user should gradually understand what they are creating, why it exists, what can go wrong, and how to ask Codex for future changes safely.

Assume the user is on Windows unless they clearly say otherwise. Explain when a command is meant for Windows PowerShell, and distinguish local Windows commands from remote Linux server commands.

## Explanation Loop

For each meaningful phase, use this loop:

1. **Concept**: explain the key idea in 2-5 sentences with a concrete analogy tied to the current task.
2. **Decision**: say what choice is being made now and what alternatives exist.
3. **Action**: run or propose the smallest safe step.
4. **Observation**: show what output, file, URL, or AWS console page confirms success.
5. **Recap**: state what the user just learned and what remains uncertain.

Keep the pace practical. Do not lecture before urgent security or rollback actions; handle the safety issue, then explain.

## Adjusting Explanation Depth

Infer the user's current level from their inputs:

| Level | Signals | How to respond |
| --- | --- | --- |
| Beginner | asks basic "what is" questions, cannot interpret terminal output, is unsure what account/region/domain means | Use plain language, define every new term, keep steps small, ask one question at a time |
| Working | can repeat concepts, understands files/commands, asks tradeoff questions | Give concise explanations, include why the choice matters, invite them to choose between safe options |
| Advanced | names services correctly, asks about least privilege, rollback, cost, CI, architecture | Reduce basics, focus on risks, tradeoffs, constraints, and verification |

Update the inferred level as the user demonstrates understanding. Do not label the user in a patronizing way; say "I'll keep this high-level" or "I'll explain this term because it matters for the next step."

## Micro-Checks

Use quick checks instead of formal quizzes:

- "Before I continue: do you want a one-sentence explanation of this term, or should I proceed?"
- "You do not need to answer perfectly, but can you tell whether this step creates a paid resource or only edits local files?"
- "This output proves the AWS profile works because it returns account identity without showing secrets."

If the user is confused, slow down and restate the concept using the current project. If they are comfortable, continue and keep explanations shorter.

## Core Concept Cards

Use these short cards as needed. Do not dump all of them at once.

### AWS Account

An AWS account is the billing, identity, and resource boundary. Everything billable or permissioned lives inside an account. For beginners, first confirm which account is being used so Codex does not create resources in the wrong place.

### Root User

The root user is the account owner identity. It can do nearly everything, so it should be protected with MFA and used rarely. Do not create root access keys.

### IAM User, Role, and Policy

IAM controls who can do what. A user is a long-lived identity, a role is usually temporary access assumed by a person or service, and a policy is the permission document. Start broad only during bootstrap when necessary, then reduce permissions as the workflow becomes clear.

### AWS Region

A region is the geographic area where resources run. Region affects latency, cost, service availability, and sometimes legal/data expectations. Pick one deliberately and write it in docs.

### Local Machine vs Remote Server

The local machine is the user's own computer, assumed to be Windows by default. The remote server is usually a Linux EC2 instance running in AWS. Commands, file paths, and environment variable syntax differ, so say which machine each instruction is for.

### EC2

EC2 is a virtual server. It is useful when the user needs a machine that keeps running Docker services, background jobs, or a reverse proxy. It costs money while running, and it needs patching, backups, and network rules.

### Security Group

A security group is the network firewall around AWS resources like EC2. Opening `80` and `443` is common for web traffic; opening admin ports broadly is risky. Always connect a port rule to a specific service need.

### Docker

Docker packages an app and its runtime into an image, then runs it as a container. Containers make deployments repeatable, but data must live in volumes or external stores if it should survive restarts.

### Docker Compose

Docker Compose describes multiple containers in one file. It is a good fit for small projects on one server. Use explicit project names, service names, volumes, and networks so one app does not disturb another.

### Caddy and Reverse Proxy

Caddy can receive HTTPS traffic and forward it to the correct app container. The reverse proxy is the front door: changing it can affect every domain it serves, so preserve marked blocks and verify neighbors.

### DNS and Route 53

DNS maps a domain name to a target like an IP address or load balancer. Route 53 is AWS's DNS service. DNS changes can take time to propagate, and mistakes can make a site unreachable.

### Registrar and Nameserver Delegation

A registrar is where the user buys and renews a domain. Route 53 can manage DNS only after the domain's registrar points the domain at Route 53's nameservers. For beginners, explain that buying a domain and managing DNS are related but separate jobs.

### ICP Filing

ICP filing is a mainland China website compliance workflow that can apply when a site is hosted on mainland China infrastructure. For a first overseas-hosted project, avoid turning this into a legal deep dive; explain that mainland hosting is a separate planning track and should not be assumed.

### S3

S3 stores objects such as deployment bundles, backups, and static files. Buckets and prefixes need ownership rules and lifecycle policies. Avoid bucket-wide cleanup unless the bucket exists only for that one purpose.

### Terraform

Terraform keeps infrastructure in code. A plan shows what will change before applying. Treat deletion, replacement, IAM broadening, DNS changes, and lifecycle changes as review points.

### SSM

AWS Systems Manager lets Codex operate an EC2 instance without opening SSH. It is usually safer for routine remote commands because access is tied to AWS identity and can be audited.

### Git Commit

A commit is a named snapshot of code and docs. Good commits make work reviewable and reversible. Commit after verified, coherent changes.

### Branch

A branch is an isolated line of work. Feature branches let Codex make changes without destabilizing the default branch. Merge after review and verification.

### Test and Verification

Tests check behavior; verification checks the whole delivery contract. A project should have one default command that Codex can run before committing or deploying.

### CI

Continuous integration runs checks automatically on GitHub or another service. CI catches problems outside the local machine and protects important branches.

### AGENTS.md

`AGENTS.md` is the instruction manual for future Codex sessions. Put hard rules, context order, and verification commands there so the next session does not rely on memory or chat history.

### Codebase-Memory MCP

Codebase-Memory MCP gives Codex a graph of code symbols and relationships. It helps find functions, routes, and call paths without repeatedly scanning files. Use plain file search for docs, configs, literals, and error messages.

## Learning Notes

For long onboarding, create or update `docs/learning-notes.md` only when useful. Keep it short:

```markdown
# Learning Notes

## Current Level

- AWS: beginner
- Git/GitHub: beginner
- Server/Docker: beginner
- Project docs/testing: beginner

## Concepts Covered

- YYYY-MM-DD: AWS account, root user, IAM profile

## Still Confusing

- <topic>

## Explanation Preference

- Use short concept explanations before live AWS or GitHub changes.
```

Do not put secrets, account IDs, access keys, tokens, or private infrastructure details in learning notes.
