# Integration Strategy

TrustForge should grow as an **ecosystem of evidence providers**, not a monolith.

## Core idea

A provider is an adapter that turns an existing tool's output into a TrustForge evidence envelope.

```text
Existing Tool
    │
    ▼
Provider Adapter
    │
    ▼
Normalized Evidence
    │
    ▼
TrustForge Orchestrator
```

The provider does not need to become TrustForge-aware internally.

## Provider categories

Initial categories:

- `context`
- `ci`
- `static_analysis`
- `dependency`
- `policy`
- `provenance`
- `runtime`
- `supply_chain_context`
- `reasoning`

See [../specs/evidence-provider.md](../specs/evidence-provider.md).

## Execution classes

Every provider declares an execution class.

### `data_only`

Safe to run in the trusted control plane only if it never executes contribution-controlled code.

Examples may include:
- reading forge metadata;
- parsing a diff with a memory-safe parser;
- reading trusted base policy.

### `isolated_required`

The provider can trigger or load contribution-controlled executable behavior.

Examples:
- dependency installation;
- build/test execution;
- project plugins;
- package lifecycle hooks;
- language tooling that loads project code.

These providers MUST run through `IsolationProvider`.

### `external_service`

The provider sends data outside the local trust boundary.

It must declare:
- transmitted data;
- authentication;
- destination;
- privacy/retention implications;
- whether repository policy permits use.

## Provider selection

TrustForge should not run every provider on every PR.

Provider selection may depend on:

- files/subsystems changed;
- dependency manifest changes;
- security-sensitive paths;
- project policy;
- language/ecosystem;
- existing CI availability;
- cost/latency;
- whether the provider requires external data transfer.

Example:

```text
PR only changes docs
   ↓
context + policy providers
   ↓
skip dependency/build providers

PR changes auth + dependencies
   ↓
context
+ static analysis
+ dependency intelligence
+ provenance
+ isolated build/tests
+ security reviewer routing
```

## Raw evidence stays available

Normalization must not erase provider detail.

Each normalized result should retain:
- provider ID/version;
- raw artifact or URL reference;
- source revision;
- execution environment;
- timestamp;
- policy revision where applicable.

A maintainer must be able to drill down from the TrustForge summary into the original result.

## Provider failure semantics

Provider failure is not automatically contribution failure.

Distinguish:

- `provider_unavailable`
- `provider_error`
- `provider_timeout`
- `evidence_not_applicable`
- `evidence_inconclusive`
- `check_failed`

Project policy determines whether missing evidence is:
- tolerated;
- `NEEDS_REVIEW`;
- or a hard `POLICY_FAILED`.

## Initial provider priorities

The first vertical slice should prefer mature, understandable integrations:

1. existing GitHub/CI results;
2. OpenShell isolation;
3. OSV-Scanner;
4. Semgrep;
5. OPA/Conftest;
6. optional provenance via Sigstore/in-toto.

Later candidates:
- GitHub Dependency Review;
- OpenSSF Scorecard;
- GUAC;
- ORT;
- CI runtime telemetry;
- optional PR reasoning tools.

## Anti-lock-in rule

No provider becomes a mandatory semantic dependency of the portable evidence model.

Provider-specific fields belong under an extension/raw namespace unless they represent a cross-provider concept worth standardizing.

## Success measure for an integration

A provider adapter is successful when:

- it adds useful evidence without duplicating the underlying engine;
- its findings remain traceable;
- its failure semantics are clear;
- it does not flood the maintainer with raw output;
- the evidence can participate in project policy without hiding uncertainty.
