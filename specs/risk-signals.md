# Spec: Risk signals

Status: **draft** (v0)

## Purpose

Surface review-relevant risk in an explainable form. Risk signals highlight where human attention is needed; they do not assert exploitability or approve/deny a change.

## Example signals (v0)

| Signal | Description |
| --- | --- |
| `security_sensitive_files` | Touches paths historically or policy-marked as security-sensitive |
| `authentication_logic_change` | Diff affects authentication/authorization logic |
| `dependency_change` | Manifest/lockfile or dependency graph changes |
| `build_system_change` | Build, CI, or packaging definitions changed |
| `privileged_operations` | Privilege escalation paths, secrets handling, or admin surfaces |
| `unexpected_generated_or_binary` | Unexpected binaries or large generated artifacts |
| `large_blast_radius` | Unusually broad subsystem or cross-cutting impact |

## Representation

In the evidence object, risk signals may appear as booleans or structured objects. Preferred future form:

```yaml
risk:
  signals:
    - id: authentication_logic_change
      present: true
      explanation: "Diff modifies token validation in auth/session.go"
      evidence_refs:
        - path: auth/session.go
```

Until the structured form is stabilized, the flat boolean map in the README example is acceptable for prototypes.

## Normative rules

1. Every `present: true` signal SHOULD include an explanation (required once structured form is adopted).
2. Absence of signals MUST NOT be interpreted as "safe."
3. Signals MUST NOT encode contributor demographic attributes.
4. Signals are inputs to human review, not merge predicates.
