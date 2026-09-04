# Spec: Review evidence

Status: **draft** (v0)

## Purpose

Define the auditable package presented to a human reviewer.

The review-evidence package is the primary TrustForge output.

## Top-level shape

```yaml
schema_version: trustforge.dev/review/v0alpha1

contribution: {}
context: {}

providers:
  runs: []

isolation: {}

evidence:
  items: []

policy: {}

risk: {}

review:
  required_expertise: []
  unresolved_questions: []

provenance: {}
```

## Provider runs

TrustForge records which providers contributed evidence.

```yaml
providers:
  runs:
    - id: osv-scanner
      category: dependency
      status: ok
      artifact_ref: "<raw-result>"

    - id: semgrep
      category: static_analysis
      status: ok
      artifact_ref: "<raw-result>"
```

See [evidence-provider.md](evidence-provider.md).

## Evidence items

Each item records:
- provider;
- evidence class;
- kind;
- result;
- exact contribution revision;
- raw artifact/reference;
- execution/isolation context when relevant.

Example:

```yaml
evidence:
  items:
    - provider: osv-scanner
      class: verified
      kind: dependency_vulnerability
      result: no_known_vulnerability
      subject: "<dependency>"

    - provider: trustforge-context
      class: inferred
      kind: architectural_fit
      result: needs_review
      confidence: medium
```

## Evidence classes

### Verified
Deterministic/reproducible provider output with provenance.

### Observed
Fact from repository or runtime state.

### Inferred
Reasoned conclusion with evidence and uncertainty.

### Human
Maintainer/reviewer judgment or attestation.

## Isolation section

Records the environment in which contributor-controlled behavior was evaluated.

| Field | Meaning |
| --- | --- |
| `provider` | Isolation provider ID |
| `provider_version` | Runtime/provider version |
| `non_root` | Whether contribution code ran without root privilege |
| `network_default` | `deny`, `restricted`, `unknown`, etc. |
| `blocked_egress_attempts` | Count/references |
| `credentials_exposed` | Must be `false` for compliant POC |
| `policy_revision` | Trusted isolation/policy revision |

## Policy section

The policy decision is one of:

- `PASS`
- `NEEDS_REVIEW`
- `POLICY_FAILED`

`POLICY_FAILED` requires an explicit trusted project rule plus deterministic/provider evidence.

Inferred/model findings alone MUST NOT produce `POLICY_FAILED`.

## Provider failure semantics

Missing evidence must remain distinguishable from failed evidence.

Example:

```yaml
providers:
  runs:
    - id: semgrep
      status: provider_timeout
```

Project policy determines whether that missing evidence requires:
- retry;
- `NEEDS_REVIEW`;
- or `POLICY_FAILED`.

## Normative rules

1. Evidence packages MUST NOT contain a single opaque universal trust score.
2. Verified/observed/inferred/human evidence MUST remain distinguishable.
3. Model commentary MUST NOT be labeled as deterministic validation.
4. The package MUST identify the exact contribution revision.
5. Provider identity and raw evidence reference SHOULD be preserved.
6. Isolation evidence MUST identify the isolation provider and policy revision.
7. Hard `POLICY_FAILED` decisions MUST trace to trusted explicit policy and deterministic evidence.
8. Provider unavailability MUST NOT be misrepresented as a contribution failure.
9. Contribution-controlled code MUST NOT be trusted to self-attest successful validation without trusted provider provenance.
