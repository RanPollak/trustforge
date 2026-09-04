# Architecture (draft)

TrustForge is an **evidence-orchestration layer for software contributions**.

Its architecture intentionally delegates specialized analysis and isolation to existing tools.

## System view

```text
                       Untrusted Contribution
                                │
                                ▼
                     ┌────────────────────┐
                     │ Context + Triage   │
                     └─────────┬──────────┘
                               │
                Trusted base policy / config
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Provider Orchestrator│
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
  Data-only providers   Isolated providers   External providers
  metadata/history      build/test/scans     remote services/APIs
          │                    │                    │
          │            ┌───────▼────────┐           │
          │            │ IsolationProvider│          │
          │            │ OpenShell POC   │           │
          │            └───────┬────────┘           │
          │                    │                    │
          └────────────────────┼────────────────────┘
                               ▼
                    ┌─────────────────────┐
                    │ Evidence Normalizer │
                    └──────────┬──────────┘
                               │
                 VERIFIED / OBSERVED /
                   INFERRED / HUMAN
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Policy Decision     │
                    │ PASS                │
                    │ NEEDS_REVIEW        │
                    │ POLICY_FAILED       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Maintainer Report   │
                    └─────────────────────┘
```

## What belongs in TrustForge core

### Contribution context
Binds all evidence to:
- repository;
- base revision;
- head revision;
- PR/patch identity;
- changed files/subsystems;
- relevant project ownership and policy revision.

### Provider orchestration
Chooses which evidence providers are required based on:
- project policy;
- change type;
- affected subsystem;
- security sensitivity;
- provider availability/capabilities.

### Evidence normalization
Maps provider-specific output into a portable envelope while preserving:
- provider identity/version;
- evidence class;
- subject;
- source revision;
- raw artifact/reference;
- confidence/uncertainty where applicable;
- execution environment and isolation provenance.

### Policy decision
Maps explicit project requirements and normalized evidence to:
- `PASS`;
- `NEEDS_REVIEW`;
- `POLICY_FAILED`.

### Maintainer presentation
Shows:
- what is known;
- where each result came from;
- which policy applies;
- what failed;
- what remains uncertain;
- which human expertise is needed.

## What does not belong in TrustForge core

TrustForge should not become:

- a SAST engine;
- a vulnerability database/scanner;
- a policy language;
- a signing/provenance implementation;
- an SBOM engine;
- a sandbox runtime;
- a general AI code-review engine;
- a CI system.

Those capabilities belong behind provider interfaces.

## Provider categories

Initial categories:

| Category | Example evidence |
| --- | --- |
| `context` | owners, history, linked issue, change metadata |
| `ci` | test/check results |
| `static_analysis` | code/security findings |
| `dependency` | vulnerability/license/source findings |
| `policy` | deterministic policy evaluation |
| `provenance` | signatures/attestations |
| `runtime` | network/process/filesystem observations |
| `reasoning` | inferred architecture/scope findings |
| `supply_chain_context` | artifact/dependency relationships |

Example candidate providers include Semgrep, OSV-Scanner, OPA/Conftest, Sigstore, in-toto, GUAC, existing CI systems, ORT, and optional PR reasoning tools.

Candidate integrations are documented in [ecosystem-map.md](ecosystem-map.md).

## Provider execution classes

Every provider declares how it can safely run.

### `data_only`
Consumes trusted API metadata, a raw diff, or other inputs without executing contribution-controlled code.

### `isolated_required`
May install packages, load project plugins, run project code, execute build/test logic, or otherwise interact with attacker-controlled executable behavior.

It MUST run behind an `IsolationProvider`.

### `external_service`
Sends data to a remote service.

It MUST declare:
- what data leaves the trust boundary;
- authentication requirements;
- retention/privacy implications;
- whether project policy permits that provider.

## Isolation boundary

OpenShell is the **reference isolation provider for the first POC**, not a core dependency.

The TrustForge core interacts with a portable `IsolationProvider` contract.

Anything that can trigger contribution-controlled execution must never silently fall back to the host if isolation is unavailable.

See [isolation-model.md](isolation-model.md) and [../specs/isolation-provider.md](../specs/isolation-provider.md).

## Evidence trust classes

- **Verified** — deterministic/reproducible result with provider provenance.
- **Observed** — fact observed from repository/runtime state.
- **Inferred** — reasoned conclusion with evidence and uncertainty.
- **Human** — maintainer/reviewer decision or attestation.

An inferred result cannot be promoted to verified solely because multiple models agree.

## Policy semantics

- **`PASS`** — all required deterministic checks passed.
- **`NEEDS_REVIEW`** — no hard violation was proven, but human judgment is required.
- **`POLICY_FAILED`** — one or more explicit machine-evaluable repository policies failed.

A hard failure must trace to:
1. trusted project policy;
2. deterministic/provider evidence;
3. exact contribution revision.

## First POC architecture

```text
GitHub PR
   │
   ├── GitHub metadata / CODEOWNERS
   ├── existing CI
   ├── Semgrep          (candidate)
   ├── OSV-Scanner      (candidate)
   ├── OPA/Conftest     (candidate)
   └── OpenShell        (reference isolation provider)
              │
              ▼
      EvidenceProvider envelopes
              │
              ▼
        TrustForge core
              │
              ▼
       Maintainer report
```

The POC succeeds only if TrustForge's value is visible **between** these tools: normalization, orchestration, policy semantics, provenance, and human-focused evidence.

## Architectural constraints

1. No TrustForge component receives authority to approve or merge a contribution.
2. Contributor-controlled execution requires an isolation provider.
3. Isolation failure fails closed.
4. Model/inferred findings cannot independently create hard merge-blocking decisions.
5. Hard decisions trace to trusted project policy and deterministic evidence.
6. A PR cannot change the effective policy used for its own current evaluation.
7. Provider-specific output is preserved for auditability but does not leak into the portable core schema unnecessarily.
8. New engines are disfavored when a provider adapter can use a mature existing project.
