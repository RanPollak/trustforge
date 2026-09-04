# Security Policy

## Supported versions

TrustForge is pre-alpha. Security reports are welcome for:

- this repository's documentation, schemas, examples, and prototypes;
- any published review-evidence formats that could be misused to forge or obscure evidence;
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

## TrustForge-specific notes

TrustForge treats **security as a first-class review dimension**. Reports that show how evidence objects could be spoofed, stripped, or misinterpreted by maintainers are especially valuable.

Do **not** propose or contribute changes that:

- present LLM opinion as security evidence;
- bypass required human review;
- silently weaken deterministic validation signals.
