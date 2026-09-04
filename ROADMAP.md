# Roadmap

TrustForge is in **exploration / pre-alpha**. The goal of early milestones is to validate the problem and define an interoperable review-evidence model — not to ship a large AI review system.

## Milestone 0 — Problem & principles (current)

- [x] Public problem statement and vision
- [x] Core principles documented
- [ ] Maintainer interviews and workflow capture
- [ ] Evidence baseline in `research/ecosystem-evidence.md`
- [ ] Shared vocabulary for context, triage, validation, isolation, policy, risk, and trust evidence

## Milestone 1 — Review-evidence & policy model

- [ ] Draft schemas for contribution context, risk signals, review evidence, and policy decisions
- [ ] JSON Schema / YAML examples under `specs/` and `examples/`
- [ ] Mapping from common CI, static analysis, dependency, provenance, and policy outputs into evidence fields
- [ ] Explicit separation between verified facts, observed facts, inferred findings, and human decisions
- [ ] Explicit non-goals and anti-patterns documented in the trust model

## Milestone 2 — Context & triage prototype

- [ ] Prototype that assembles context from a single pull request
- [ ] Resolve CODEOWNERS and likely subsystem ownership
- [ ] Identify related issues/PRs and historical changes
- [ ] Surface broad-scope, duplicate, or missing-context signals
- [ ] Produce a human-readable evidence summary suitable for maintainer review

## Milestone 3 — Isolated PR evaluation & policy POC

Reference isolation runtime: **OpenShell**.

- [ ] Create an ephemeral sandbox for the exact PR revision
- [ ] Run install/build/test/scanners inside the isolation boundary
- [ ] Run PR-controlled code without ambient host credentials
- [ ] Enforce default-deny or tightly scoped network egress
- [ ] Capture denied network/runtime behavior as evidence
- [ ] Enforce explicit dependency source/registry policy
- [ ] Distinguish `PASS`, `NEEDS_REVIEW`, and `POLICY_FAILED`
- [ ] Expose hard deterministic policy failures as a repository status/check result
- [ ] Demonstrate that model-only findings cannot hard-block a PR

Success criteria:
- malicious build/test behavior cannot access host secrets;
- unauthorized egress is blocked and observable;
- a policy failure is explainable and tied to a specific repository rule;
- architecture/scope findings remain advisory unless explicitly codified.

## Milestone 4 — Validation & risk aggregation

- [ ] Attach deterministic validation artifacts (CI, tests, linters)
- [ ] Integrate static analysis and dependency scanners
- [ ] Capture provenance/signature evidence
- [ ] Surface security-sensitive paths and privilege/authentication changes
- [ ] Estimate blast radius and unresolved evidence

## Milestone 5 — Evaluation

- [ ] Define maintainer-effort metrics (time-to-first-meaningful-review, active review time, rework rate, noise reduced)
- [ ] Small evaluation set of real or anonymized contributions
- [ ] Compare evidence-assisted review vs. baseline maintainer workflow
- [ ] Measure false-positive/advisory burden
- [ ] Publish methodology and findings under `research/`

## Milestone 6 — Integrations (post-validation)

- [ ] GitHub Action / check-run that publishes a review-evidence artifact
- [ ] Optional integrations with CODEOWNERS, dependency scanners, signing/provenance tools, and policy engines
- [ ] Isolation-runtime adapter interface beyond the OpenShell reference POC
- [ ] Interoperability notes for other forges and review tools

## Non-goals (near term)

- Automatic approve/merge
- Automatically closing PRs based on probabilistic judgment
- Opaque universal trust scores
- Replacing project governance or required human review
- Using LLM opinion as security evidence
- Executing untrusted contribution code outside a dedicated isolation boundary
