# Roadmap

TrustForge is in **exploration / pre-alpha**.

The roadmap is deliberately structured to avoid rebuilding mature open-source capabilities. TrustForge should first define the missing contracts and compose existing tools into a useful maintainer workflow.

## Milestone 0 — Problem, landscape & principles

- [x] Public problem statement and vision
- [x] Core maintainer-first principles
- [x] Isolation and policy models
- [x] Initial open-source ecosystem/duplication map
- [ ] Maintainer interviews and workflow capture
- [ ] Complete evidence baseline in `research/ecosystem-evidence.md`
- [ ] Validate the proposed TrustForge boundary with maintainers and adjacent projects

Exit criterion:

> We can explain in one minute what TrustForge owns, what it integrates, and why it is not another PR-review bot.

## Milestone 1 — Core interoperability contracts

Define the small TrustForge core before building integrations.

- [ ] Stabilize contribution-context vocabulary
- [ ] Draft normalized review-evidence schema
- [ ] Draft `EvidenceProvider` contract
- [ ] Draft `IsolationProvider` contract
- [ ] Draft policy-decision semantics
- [ ] Define evidence classes: verified, observed, inferred, human
- [ ] Define provenance requirements for provider results
- [ ] Define provider execution classes (`data_only`, `isolated_required`, `external_service`)
- [ ] Add JSON Schema/YAML examples under `specs/` and `examples/`

Exit criterion:

> Two unrelated tools can produce evidence through the provider contract without changing the TrustForge core schema.

## Milestone 2 — First composition vertical slice

Build the smallest end-to-end workflow that proves TrustForge is an **orchestrator/evidence layer**, not a replacement for existing tools.

Reference/candidate stack:

- GitHub PR metadata + CODEOWNERS
- existing repository CI/test results
- **OpenShell** as the reference isolation provider
- **OSV-Scanner** as a dependency-vulnerability evidence provider
- **Semgrep** as a static-analysis evidence provider
- **OPA/Conftest** as a deterministic policy provider

Optional if readily available:

- Sigstore / in-toto provenance evidence

Deliverables:

- [ ] Load trusted base policy
- [ ] Build contribution context for exact PR revision
- [ ] Select providers based on policy and change type
- [ ] Execute contributor-controlled steps only through isolation
- [ ] Normalize all provider outputs into one evidence package
- [ ] Produce `PASS`, `NEEDS_REVIEW`, or `POLICY_FAILED`
- [ ] Generate one concise maintainer-facing report
- [ ] Preserve raw provider artifacts/references for auditability

Exit criterion:

> The demo uses at least three independent existing tools, and TrustForge adds value by composing their evidence rather than reproducing their analysis.

## Milestone 3 — Isolation & adversarial POC

Harden the isolated-evaluation path.

- [ ] Demonstrate no ambient host credentials are exposed
- [ ] Demonstrate unauthorized egress is denied and recorded
- [ ] Demonstrate malicious install/build/test behavior is contained
- [ ] Demonstrate a PR cannot weaken the policy used for its own run
- [ ] Fail closed if the isolation provider cannot establish required controls
- [ ] Re-check relevant OpenShell security/trust-boundary issues before widening workload-image support
- [ ] Bind runtime evidence to exact contribution revision and isolation-provider version

Exit criterion:

> A deliberately malicious fixture is contained, policy violations are auditable, and isolation failure never silently falls back to host execution.

## Milestone 4 — Context, triage & optional reasoning

Add the higher-value context that scanners do not provide.

- [ ] Resolve CODEOWNERS and likely subsystem ownership
- [ ] Identify related issues/PRs and historical changes
- [ ] Surface missing context, broad-scope changes, and likely duplicates
- [ ] Detect architecture/library-fit concerns
- [ ] Define optional reasoning-provider interface
- [ ] Keep inferred findings separate from deterministic evidence
- [ ] Route inferred findings to `NEEDS_REVIEW`, never hard failure by model opinion alone

Exit criterion:

> Maintainers receive useful project context without being flooded by generated commentary.

## Milestone 5 — Provider ecosystem

Prioritize adapters over new engines.

Candidate adapters:

- [ ] OpenShell
- [ ] existing CI / GitHub Checks
- [ ] Semgrep
- [ ] OSV-Scanner
- [ ] OPA / Conftest
- [ ] GitHub Dependency Review
- [ ] OpenSSF Scorecard
- [ ] Sigstore
- [ ] in-toto
- [ ] GUAC
- [ ] ORT
- [ ] optional PR reasoning/review tools

Each adapter must document:

- evidence category;
- execution/trust requirements;
- raw artifact provenance;
- failure semantics;
- what TrustForge does **not** infer from the provider result.

## Milestone 6 — Maintainer evaluation

Measure the project against the actual problem.

- [ ] Time to first meaningful review
- [ ] Active maintainer review time per PR
- [ ] Queue age
- [ ] Number of tools/pages a maintainer must inspect
- [ ] False-positive/advisory burden
- [ ] Percentage of evidence accepted as useful
- [ ] Time to trusted merge
- [ ] CI cost for rejected/noisy contributions

Compare:

> baseline maintainer workflow vs. TrustForge-assisted workflow.

Publish methodology and findings under `research/`.

## Milestone 7 — Forge integrations

Only after the evidence model and POC show value:

- [ ] GitHub Check / Action
- [ ] CLI
- [ ] Forge-neutral webhook/event model
- [ ] GitLab / Forgejo interoperability notes
- [ ] Provider SDK/documentation

## Non-goals

- Building another general-purpose SAST engine
- Building another dependency vulnerability database/scanner
- Building another policy language when OPA/CEL/etc. can express the rule
- Building another provenance/signing system
- Building another sandbox runtime
- Building another general AI PR reviewer
- Automatic approve/merge
- Automatic PR closure based on probabilistic judgment
- Opaque universal trust scores
- Replacing project governance or required human review
- Executing untrusted contribution code outside a dedicated isolation boundary
