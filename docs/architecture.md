# Architecture (draft)

TrustForge v0 is a **review-evidence and policy-enforcement pipeline**, not a merge bot.

```text
                    Untrusted Contribution
                              │
                              ▼
                    ┌──────────────────┐
                    │ Context Builder  │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │      Triage      │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Policy Precheck  │
                    └────────┬─────────┘
                             │
                             ▼
        ╔══════════════════════════════════════════╗
        ║        ISOLATED EVALUATION ZONE          ║
        ║                                          ║
        ║  Reference runtime for POC: OpenShell    ║
        ║                                          ║
        ║  checkout / install / build / test       ║
        ║  static analysis / scanners / probes     ║
        ║                                          ║
        ║  restricted filesystem                   ║
        ║  non-root process                        ║
        ║  syscall/process restrictions            ║
        ║  controlled/default-deny network egress  ║
        ║  no ambient host credentials             ║
        ╚═══════════════════╤══════════════════════╝
                            │
                            ▼
                  ┌──────────────────────┐
                  │ Validation + Risk    │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Policy Enforcement   │
                  │ PASS / NEEDS_REVIEW  │
                  │ / POLICY_FAILED      │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   Evidence Builder   │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │ Human Maintainer     │
                  │ Decision             │
                  └──────────────────────┘
```

## Trust boundaries

### Trusted control plane
Owns repository policy, orchestration, evidence schemas, and final TrustForge status. It must never treat executable state from the contribution as trusted.

### Untrusted contribution
The PR diff, checkout, test code, build scripts, dependency manifests, generated files, and packages fetched because of the PR are potentially attacker-controlled.

### Isolated evaluation boundary
Anything that executes contributor-controlled behavior runs inside a restricted disposable environment.

For the first POC, TrustForge uses **OpenShell** as the reference runtime. OpenShell documents overlapping isolation controls including an unprivileged child process, Landlock filesystem restrictions, seccomp restrictions, a network namespace, and policy-controlled egress.

TrustForge should expose an `IsolationRuntime` interface so future runtimes can implement the same contract without changing the public evidence model.

## Components (conceptual)

| Stage | Responsibility | Inputs (examples) | Outputs |
| --- | --- | --- | --- |
| Context | Compact, traceable picture of the change | Diff, commits, linked issues/PRs, CODEOWNERS, history | Contribution context object |
| Triage | Route and filter | Context, size/breadth heuristics, ownership | Triage hints (duplicate, missing context, specialist needs) |
| Policy precheck | Apply cheap explicit rules before execution | Trusted base policy, paths, dependency manifests, ownership | Precheck pass/fail/review findings |
| Isolation | Contain contributor-controlled execution | Exact PR revision, runtime policy | Runtime evidence, denial events, artifacts |
| Validation | Collect deterministic evidence | CI, tests, linters, SAST, deps, policy, signing | Validation results with provenance |
| Risk analysis | Surface review-relevant risk | Paths, change types, runtime events, dependency/build diffs | Explained risk signals |
| Policy enforcement | Map explicit rules to outcome | Deterministic evidence + policy | PASS / NEEDS_REVIEW / POLICY_FAILED |
| Trust evidence | Assemble auditable package | All prior stages | Review-evidence artifact for humans/tools |

## Policy semantics

- **`PASS`** — all required deterministic checks passed.
- **`NEEDS_REVIEW`** — no hard policy violation was proven, but human judgment is required.
- **`POLICY_FAILED`** — one or more explicit machine-evaluable repository policies failed.

A `POLICY_FAILED` result can be exposed as a failing repository status/check so normal branch protection can block merge. TrustForge v0 does not automatically close the PR or make the merge decision itself.

## Deployment shapes (later)

Possible thin integrations after the model stabilizes:

- GitHub Action / check-run that posts or attaches an evidence artifact
- CLI that generates evidence from a local checkout + CI outputs
- Library adapters that map tool outputs into the evidence schema
- Isolation-runtime adapters, with OpenShell as the first reference POC

## Architectural constraints

1. No component in v0 receives authority to approve or merge a contribution.
2. Contributor-controlled code is never executed outside the isolation boundary.
3. An LLM finding alone cannot create a hard merge-blocking decision.
4. Hard failures must trace back to an explicit repository policy or deterministic required check.
5. Runtime isolation must be observable: policy denials and attempted boundary violations become review evidence.
6. Policy used to evaluate a PR must come from a trusted base/control plane, not the untrusted PR revision.

## Non-architecture (v0)

- No central authority that certifies contributors
- No required cloud service for the evidence model itself
- No automatic merge controller
- No OpenShell-specific public TrustForge schema; isolation is an adapter boundary

See also [trust-model.md](trust-model.md), [isolation-model.md](isolation-model.md), [policy-model.md](policy-model.md), [threat-model.md](threat-model.md), and specs under [`../specs/`](../specs/).
