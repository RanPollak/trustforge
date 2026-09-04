# Open Source Ecosystem Map

TrustForge intentionally sits **between** existing review/security tools and the maintainer decision.

This document exists to prevent unnecessary duplication.

## Positioning

> **TrustForge is not another code-review tool. It is an open evidence layer that composes review, security, policy, provenance, and isolated-execution signals around an exact software contribution.**

## Adjacent projects and planned relationship

| Project | What it already does well | Overlap with TrustForge | TrustForge stance |
| --- | --- | --- | --- |
| [PR-Agent](https://github.com/qodo-ai/pr-agent) | AI-assisted PR description/review/improvement | Context and reasoning | Optional reasoning provider; do not rebuild a general AI reviewer |
| [Danger](https://github.com/danger/danger) | Automates repository/PR review chores and rules | PR policy/triage | Reuse or consume results where useful |
| [OSS Review Toolkit (ORT)](https://github.com/oss-review-toolkit/ort) | Dependency/compliance analysis, policy automation, orchestration, reporting | Strong architectural adjacency in dependency/compliance workflows | Integrate relevant evidence; keep TrustForge centered on incoming contribution trust |
| [OpenShell](https://github.com/NVIDIA/OpenShell) | Sandboxed execution and runtime policy | Isolation/runtime evidence | Reference `IsolationProvider` for first POC |
| [Harden-Runner](https://github.com/step-security/harden-runner) | Observes/controls CI runtime behavior in supported modes | Runtime evidence | Potential runtime evidence provider; do not recreate CI telemetry |
| [OPA / Conftest](https://github.com/open-policy-agent/conftest) | Policy-as-code over structured data | Deterministic policy | Candidate policy provider; do not invent a new policy language without a demonstrated gap |
| [Semgrep](https://github.com/semgrep/semgrep) | Static analysis and pattern-based security analysis | Validation/risk | Candidate static-analysis provider |
| [GitHub Dependency Review](https://github.com/actions/dependency-review-action) | PR dependency-diff vulnerability/license checks | Dependency policy | Candidate provider on GitHub; do not recreate dependency diffing |
| [OSV-Scanner](https://github.com/google/osv-scanner) | Open-source dependency vulnerability analysis | Dependency risk | Candidate dependency provider |
| [OpenSSF Scorecard](https://github.com/ossf/scorecard) | Open-source project security-health signals | Dependency/project posture | Optional posture evidence provider |
| [GUAC](https://github.com/guacsec/guac) | Aggregates supply-chain metadata into a graph | Context/evidence aggregation | Potential supply-chain context provider |
| [in-toto](https://github.com/in-toto/in-toto) | Supply-chain integrity metadata and verification | Provenance | Candidate provenance provider |
| [Sigstore Policy Controller](https://github.com/sigstore/policy-controller) | Policy based on verifiable supply-chain metadata | Provenance/policy | Related provider/reference for verifiable evidence |

This is not an exhaustive list.

## Where TrustForge should be unique

TrustForge should concentrate innovation in these areas:

### 1. Contribution-bound evidence
All evidence is tied to the exact incoming contribution/revision being considered.

### 2. Cross-tool evidence normalization
Different tools express findings differently.

TrustForge provides a portable evidence envelope without pretending all signals have the same trust semantics.

### 3. Evidence-class separation
Verified, observed, inferred, and human signals stay visibly distinct.

### 4. Orchestration around maintainer intent
Project policy determines which evidence is needed and which reviewers need attention.

### 5. Isolation-aware review
TrustForge treats execution of untrusted contribution code as a first-class trust boundary.

### 6. Explainable policy outcome
A hard failure must point to:
- the project rule;
- the provider evidence;
- the exact contribution revision.

### 7. Maintainer-focused presentation
The project optimizes for reduced reviewer cognitive load rather than number of generated comments.

### 8. Evaluation of review capacity
TrustForge should measure whether it reduces:
- active review time;
- queue age;
- tool-switching;
- low-value review work;
- false-positive burden.

## Build vs. integrate rule

Before adding a new engine to TrustForge core:

```text
Does an established OSS project already provide the capability?
               │
        ┌──────┴──────┐
        │             │
       YES            NO
        │             │
        ▼             ▼
Build provider     Validate that the
adapter first      gap is real
        │             │
        ▼             ▼
Normalize output   Consider a small
into evidence      new component
```

## Examples

### Static analysis

Wrong direction:

```text
TrustForge implements its own general SAST engine.
```

Preferred direction:

```text
Semgrep / project analyzer
        ↓
EvidenceProvider adapter
        ↓
TrustForge normalized evidence
```

### Policy

Wrong direction:

```text
TrustForge invents a new general policy language.
```

Preferred direction:

```text
OPA / Conftest / native project policy
        ↓
deterministic provider result
        ↓
TrustForge policy decision
```

### Isolation

Wrong direction:

```text
TrustForge grows its own container/sandbox runtime.
```

Preferred direction:

```text
OpenShell or another runtime
        ↓
IsolationProvider
        ↓
normalized runtime evidence
```

## Project-boundary test

A proposed feature belongs in TrustForge core when it primarily answers one of these questions:

- How do we bind evidence to the exact contribution?
- How do we select/run evidence providers?
- How do we normalize and classify evidence?
- How do we preserve evidence provenance?
- How do we map trusted policy + evidence to an explainable decision?
- How do we present the result to maintainers?
- How do we measure whether the workflow improves review capacity?

If it primarily answers *"How do we scan/analyze/isolate/sign this artifact?"*, it is probably a provider.
