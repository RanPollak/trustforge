# Spec: Review evidence

Status: **draft** (v0)

## Purpose

Define the auditable package presented to a human reviewer. This is the primary TrustForge output for v0.

## Top-level shape

```yaml
contribution: { ... }   # see contribution-context.md
context: { ... }
validation: { ... }
risk: { ... }            # see risk-signals.md
review: { ... }
evidence: { ... }
```

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

## Review section

| Field | Meaning |
| --- | --- |
| `required_expertise` | Tags such as `auth-subsystem`, `security`, `build-system` |

## Evidence section

Supplemental quantitative or qualitative observations that are still explainable, e.g.:

- `test_coverage_delta`
- `suspicious_patterns`
- `unresolved_findings`

## Normative rules

1. Evidence packages MUST NOT include a single opaque universal trust score.
2. Probabilistic narrative, if any, MUST be separated from deterministic validation.
3. Security-related LLM commentary MUST NOT be labeled as validation evidence.
4. The package MUST identify the contribution revision it describes.

## Example

See [../examples/evidence-pr-1234.yaml](../examples/evidence-pr-1234.yaml).
