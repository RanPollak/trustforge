# Isolated PR Evidence POC

## Goal

Prove TrustForge's differentiated value through **composition**:

> Take an untrusted PR, evaluate relevant parts through existing open-source tools inside a controlled trust boundary, normalize their evidence, apply trusted project policy, and produce one concise maintainer-facing decision package.

OpenShell is the reference isolation provider, but it is **not the entire POC**.

## Candidate stack

```text
GitHub PR
   │
   ├── metadata / CODEOWNERS
   ├── existing CI
   ├── Semgrep        → static analysis
   ├── OSV-Scanner    → dependency vulnerability evidence
   ├── OPA/Conftest   → deterministic policy
   └── OpenShell      → isolation + runtime events
             │
             ▼
      TrustForge adapters
             │
             ▼
     normalized evidence
             │
             ▼
PASS | NEEDS_REVIEW | POLICY_FAILED
             │
             ▼
       maintainer report
```

These integrations are POC targets, not claimed implemented support.

## Why this POC matters

A demo that only runs a PR in OpenShell would mostly demonstrate OpenShell.

A demo that only asks an LLM to review a diff would mostly reproduce existing PR-review tools.

A TrustForge demo must demonstrate the missing layer:

- provider selection;
- isolation-aware orchestration;
- normalized evidence;
- evidence provenance;
- trusted policy semantics;
- uncertainty separation;
- one maintainer-focused report.

## POC phases

### Phase A — Core envelopes

Implement minimal:
- contribution subject binding;
- `EvidenceProvider` envelope;
- `IsolationProvider` envelope;
- review-evidence package;
- policy decision.

### Phase B — Existing-tool composition

Connect at least:

1. existing CI/test result;
2. OSV-Scanner;
3. Semgrep;
4. OPA/Conftest;
5. OpenShell.

The POC is stronger if TrustForge writes little analysis code itself.

### Phase C — Adversarial fixtures

Use deliberately controlled malicious/edge-case fixtures.

#### A. Credential probe
A malicious test attempts to enumerate environment variables/credential locations.

Expected:
- no host secret exposed;
- runtime/isolation evidence retained.

#### B. Data exfiltration
A package hook/test attempts unauthorized outbound traffic.

Expected:
- isolation policy denies egress;
- TrustForge records runtime evidence;
- policy determines whether denial is hard failure or review.

#### C. Vulnerable dependency
PR introduces dependency with known OSV vulnerability.

Expected:
- OSV provider produces dependency evidence;
- project policy maps evidence to decision.

#### D. Untrusted dependency source
PR adds package/source outside allowed registry policy.

Expected:
- deterministic policy provider reports failure;
- TrustForge returns `POLICY_FAILED`.

#### E. Static security finding
PR adds a known pattern detected by configured Semgrep rules.

Expected:
- finding is preserved with provider provenance;
- policy controls fail/review behavior.

#### F. Architectural mismatch
PR uses a technically valid library that bypasses preferred project abstraction.

Expected:
- if not explicitly codified: `NEEDS_REVIEW`;
- reasoning/context never masquerades as verified evidence.

#### G. Provider outage
One provider times out.

Expected:
- `provider_timeout`, not a false clean result or fake PR violation;
- project policy decides next step.

#### H. Self-weakening policy change
PR modifies TrustForge/project policy.

Expected:
- current run uses trusted base policy;
- proposed policy change is reviewed as contribution data.

## OpenShell trust-boundary constraint

The first POC uses a trusted OpenShell/TrustForge base environment and checks out the PR after isolation is established.

Do not accept arbitrary PR-supplied OCI/workload images while the trust-boundary concern tracked in NVIDIA/OpenShell issue #2750 remains applicable.

Reference:
https://github.com/NVIDIA/OpenShell/issues/2750

## Success criteria

The POC succeeds when:

1. evidence from at least three independent existing tools is normalized;
2. raw provider provenance remains inspectable;
3. untrusted execution is isolated and fail-closed;
4. deterministic policy can hard-fail;
5. inferred/contextual findings cannot hard-fail by themselves;
6. provider failure is distinguishable from contribution failure;
7. the maintainer sees one concise evidence package rather than multiple disconnected tool outputs;
8. TrustForge adds visible value without implementing its own scanner/sandbox/policy language.

## Related specs

- [../../specs/evidence-provider.md](../../specs/evidence-provider.md)
- [../../specs/isolation-provider.md](../../specs/isolation-provider.md)
- [../../specs/review-evidence.md](../../specs/review-evidence.md)
- [../../specs/policy-decision.md](../../specs/policy-decision.md)
