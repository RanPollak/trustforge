# Spec: Review evidence

Status: **draft** (v0)

## Purpose

Define the auditable package presented to a human reviewer. This is the primary TrustForge output for v0.

## Top-level shape

```yaml
contribution: { ... }   # see contribution-context.md
context: { ... }
isolation: { ... }
validation: { ... }
policy: { ... }          # see policy-decision.md
risk: { ... }            # see risk-signals.md
review: { ... }
evidence: { ... }
```

## Isolation section

Records the environment in which contributor-controlled behavior was evaluated.

| Field | Meaning |
| --- | --- |
| `runtime` | Isolation runtime identifier, e.g. `openshell` for the reference POC |
| `non_root` | Whether contribution code ran without root privilege |
| `network_default` | e.g. `deny`, `restricted`, `unknown` |
| `blocked_egress_attempts` | Count or references to denied outbound attempts |
| `credentials_exposed` | Must be `false` for compliant TrustForge POC execution |
| `policy_revision` | Trusted isolation/policy revision used for the run |

Runtime-specific details should be attached as referenced evidence rather than making the TrustForge schema dependent on one sandbox implementation.

## Validation section

Deterministic results only, for example:

| Field | Meaning |
| --- | --- |
| `unit_tests` | pass / fail / not_run / unknown |
| `integration_tests` | pass / fail / not_run / unknown |
| `static_analysis` | pass / fail / not_run / unknown |
| `dependency_checks` | pass / fail / not_run / unknown |
| `policy_checks` | pass / fail / not_run / unknown |
| `provenance` | verified / missing / not_applicable / unknown |

Each validation entry SHOULD allow an optional `source` and `url` or artifact reference in future schema revisions.

## Policy section

The policy decision is one of:

- `PASS`
- `NEEDS_REVIEW`
- `POLICY_FAILED`

`POLICY_FAILED` requires at least one failed explicit deterministic required check. Inferred/model findings alone MUST NOT produce `POLICY_FAILED`.

See [policy-decision.md](policy-decision.md).

## Review section

| Field | Meaning |
| --- | --- |
| `required_expertise` | Tags such as `auth-subsystem`, `security`, `build-system` |
| `unresolved_questions` | Findings that require human judgment |

## Evidence section

Supplemental quantitative or qualitative observations that are still explainable, e.g.:

- `test_coverage_delta`
- `suspicious_patterns`
- `blocked_network_attempts`
- `untrusted_dependency_sources`
- `architectural_fit_findings`
- `unresolved_findings`

## Normative rules

1. Evidence packages MUST NOT include a single opaque universal trust score.
2. Probabilistic narrative, if any, MUST be separated from deterministic validation.
3. Security-related LLM commentary MUST NOT be labeled as validation evidence.
4. The package MUST identify the exact contribution revision it describes.
5. Isolation/runtime evidence MUST identify the policy revision used for execution.
6. A hard `POLICY_FAILED` decision MUST trace to an explicit deterministic repository policy or required check.
7. A model-only or contextual architecture/scope finding MUST remain advisory (`NEEDS_REVIEW`) unless an explicit project rule makes it deterministic.
8. Evidence produced by contributor-controlled code MUST NOT be trusted without provenance from the trusted orchestrator/runtime.

## Example

See [../examples/evidence-pr-1234.yaml](../examples/evidence-pr-1234.yaml).
