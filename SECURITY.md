# Security Policy

TrustForge operates in a security-sensitive part of the software supply chain.

## Supported versions

TrustForge is pre-alpha. Security reports are welcome for:

- this repository's documentation, schemas, examples, and prototypes;
- any published review-evidence formats that could be misused to forge or obscure evidence;
- isolation or policy bypasses in TrustForge prototypes;
- supply-chain and CI workflow issues in this project.

## Reporting a vulnerability

Please **do not** open a public GitHub issue for security vulnerabilities.

Prefer one of:

1. GitHub Security Advisories for this repository (Private vulnerability reporting), if enabled
2. Email the maintainers privately via the contact listed on the GitHub repository profile / SECURITY contact

Include:

- a description of the issue and its impact;
- steps to reproduce or a proof of concept when safe;
- affected versions, commits, or artifacts if known.

We aim to acknowledge reports within **7 days** and to provide a status update within **14 days**.

## Security design principles

- Treat repository content, pull-request content, build scripts, tests, generated files, and dependency manifests as untrusted input.
- Treat dependency installation and build/test hooks as potentially attacker-controlled execution.
- Execute contribution-controlled behavior only inside a restricted disposable environment.
- The first TrustForge POC uses **OpenShell** as its reference isolation runtime; TrustForge should remain runtime-agnostic through an adapter boundary.
- Keep ambient host, CI, repository-write, and cloud credentials out of contribution-controlled execution contexts.
- Prefer default-deny or tightly scoped outbound network access during untrusted execution.
- Use least privilege for Git hosting and policy integrations.
- Load enforcement policy from the trusted base revision/control plane so a PR cannot weaken the policy used to evaluate itself.
- Preserve evidence provenance and bind evidence to the exact contribution revision.
- Clearly distinguish verified facts from model inference.
- Never allow an LLM response alone to authorize a merge, privileged operation, or hard policy failure.

## TrustForge-specific notes

TrustForge treats **security as a first-class review dimension**. Reports that show how evidence objects could be spoofed, stripped, forged, or misinterpreted by maintainers are especially valuable.

Do **not** propose or contribute changes that:

- present LLM opinion as security evidence;
- bypass required human review;
- silently weaken deterministic validation signals;
- execute untrusted PR-controlled code directly on the TrustForge host;
- expose credentials to untrusted build/test code for convenience.

See [docs/threat-model.md](docs/threat-model.md) and [docs/isolation-model.md](docs/isolation-model.md).
