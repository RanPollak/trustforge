# Architecture (draft)

TrustForge v0 is a **pipeline that produces review evidence**, not a merge bot.

```text
Contribution
    ↓
Context
    ↓
Triage
    ↓
Validation
    ↓
Risk Analysis
    ↓
Trust Evidence
    ↓
Maintainer Decision
```

## Components (conceptual)

| Stage | Responsibility | Inputs (examples) | Outputs |
| --- | --- | --- | --- |
| Context | Compact, traceable picture of the change | Diff, commits, linked issues/PRs, CODEOWNERS, history | Contribution context object |
| Triage | Route and filter | Context, size/breadth heuristics, ownership | Triage hints (duplicate, missing context, specialist needs) |
| Validation | Collect deterministic evidence | CI, tests, linters, SAST, deps, policy, signing | Validation results with provenance |
| Risk analysis | Surface review-relevant risk | Paths, change types, dependency/build diffs | Explained risk signals |
| Trust evidence | Assemble auditable package | All prior stages | Review-evidence artifact for humans/tools |

## Deployment shapes (later)

Possible thin integrations after the model stabilizes:

- GitHub Action / check-run that posts or attaches an evidence artifact
- CLI that generates evidence from a local checkout + CI outputs
- Library adapters that map tool outputs into the evidence schema

## Non-architecture (v0)

- No central authority that certifies contributors
- No required cloud service for the evidence model itself
- No automatic merge controller

See also [trust-model.md](trust-model.md) and specs under [`../specs/`](../specs/).
