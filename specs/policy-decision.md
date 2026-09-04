# Spec: Policy decision

Status: **draft** (v0)

## Purpose

Represent the outcome of repository policy evaluation without collapsing maintainer judgment into a single opaque score.

## Shape

```yaml
schema_version: trustforge.dev/v0alpha1

policy_decision:
  status: PASS | NEEDS_REVIEW | POLICY_FAILED
  policy_revision: "<trusted-policy-revision>"

  required_checks:
    - id: dependency-source
      type: deterministic
      result: pass
      evidence_refs: []

  advisory_findings:
    - id: architectural-fit
      type: inferred
      result: needs_review
      confidence: medium
      evidence_refs: []

  runtime_violations:
    - id: network-egress
      action: denied
      destination: telemetry.example
      hard_failure: false
      evidence_refs: []
```

## Normative rules

1. `POLICY_FAILED` requires at least one failed deterministic required check.
2. Inferred/model findings alone MUST NOT produce `POLICY_FAILED`.
3. Every decision MUST point to evidence and the trusted policy revision used.
4. Runtime policy denials MUST be preserved even when they do not independently imply malicious intent.
5. `NEEDS_REVIEW` MUST be available for contextual findings such as architectural fit, scope expansion, or ambiguous runtime behavior.
6. A PR MUST NOT be able to change the effective policy used for its own current evaluation.
