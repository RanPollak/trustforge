# Roadmap

TrustForge is in **exploration / pre-alpha**. The goal of early milestones is to validate the problem and define an interoperable review-evidence model — not to ship a large AI review system.

## Milestone 0 — Problem & principles (current)

- [x] Public problem statement and vision
- [x] Core principles documented
- [ ] Maintainer interviews and workflow capture
- [ ] Evidence baseline in `research/ecosystem-evidence.md`
- [ ] Shared vocabulary for context, triage, validation, risk, and trust evidence

## Milestone 1 — Review-evidence model

- [ ] Draft schemas for contribution context, risk signals, and review evidence
- [ ] JSON Schema / YAML examples under `specs/` and `examples/`
- [ ] Mapping from common CI, static analysis, and policy outputs into evidence fields
- [ ] Explicit non-goals and anti-patterns documented in the trust model

## Milestone 2 — Thin prototypes

- [ ] Prototype that assembles context from a single pull request
- [ ] Prototype that attaches deterministic validation artifacts (CI, tests, linters)
- [ ] Prototype risk-signal surfacing for security-sensitive paths
- [ ] Human-readable evidence summary suitable for maintainer review

## Milestone 3 — Evaluation

- [ ] Define maintainer-effort metrics (time-to-first-meaningful-review, rework rate, noise reduced)
- [ ] Small evaluation set of real or anonymized contributions
- [ ] Compare evidence-assisted review vs. baseline maintainer workflow
- [ ] Publish methodology and findings under `research/`

## Milestone 4 — Integrations (post-validation)

- [ ] GitHub Action / check-run that publishes a review-evidence artifact
- [ ] Optional integrations with CODEOWNERS, dependency scanners, and signing/provenance tools
- [ ] Interoperability notes for other forges and review tools

## Non-goals (near term)

- Automatic approve/merge
- Opaque universal trust scores
- Replacing project governance or required human review
- Using LLM opinion as security evidence
