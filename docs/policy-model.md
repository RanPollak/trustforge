# Policy Model

TrustForge separates **project policy**, **provider evidence**, and **model judgment**.

TrustForge should not invent a general policy language unless existing policy engines cannot express a demonstrated requirement.

Candidate policy providers include project-native policy and tools such as OPA/Conftest.

## Policy outcomes

### PASS

All required deterministic checks passed.

### NEEDS_REVIEW

No explicit hard policy violation was proven, but one or more findings require maintainer judgment.

Examples:
- a new library appears inconsistent with established architecture;
- the change may be broader than the linked issue requires;
- a provider is unavailable and policy requires human fallback;
- an unusual implementation path has no explicit rule against it;
- a sandbox denial occurred that may be legitimate but deserves review.

### POLICY_FAILED

A machine-evaluable required project policy failed.

Examples:
- dependency source is outside allowed registries;
- protected path change is explicitly prohibited;
- required provenance/signature is missing;
- isolation/runtime behavior violates an explicit hard rule;
- required CI/security check failed.

## TrustForge does not own the policy language

```text
Project policy
    │
    ├── repository-native rules
    ├── OPA / Conftest
    ├── forge branch protection
    └── other provider
            │
            ▼
normalized policy evidence
            │
            ▼
TrustForge decision semantics
```

TrustForge owns the portable **decision model**, not necessarily the rule language.

## Hard vs. advisory

```text
Trusted explicit rule
+ deterministic evidence
          ↓
     hard enforcement
          ↓
     POLICY_FAILED

Contextual / probabilistic finding
          ↓
     advisory evidence
          ↓
      NEEDS_REVIEW
```

This distinction prevents an LLM or reasoning provider from becoming an opaque merge authority.

## Architectural/library fit

TrustForge may assess whether a contribution uses established abstractions and libraries.

Evidence can come from:
- explicit repository policy;
- architecture docs;
- dependency manifests;
- existing code patterns;
- CODEOWNERS/ownership;
- historical changes.

If a rule is explicit and machine-evaluable, a policy provider can enforce it.

If the conclusion depends on interpretation, the result remains `NEEDS_REVIEW`.

## "Unnecessary change" policy

TrustForge should help detect scope expansion, unrelated refactors, duplicated functionality, and additions that do not appear necessary for the stated issue.

Necessity is usually contextual.

Therefore:
- explicit project rule → deterministic policy result;
- inferred architecture/history/issue context → `NEEDS_REVIEW`.

## Trusted policy source

The policy used for the current evaluation comes from a trusted base revision/control plane.

A PR may propose changes to policy files, but those proposed changes are reviewed as contribution data and do not automatically govern the same run.

## Provider failures

Policy also defines what happens when evidence is missing.

Example:

```yaml
required_evidence:
  dependency_vulnerability:
    on_provider_unavailable: needs_review

  isolation:
    on_provider_unavailable: fail

  provenance:
    on_not_applicable: pass
    on_missing: needs_review
```

A provider outage and a contribution violation are different facts and should remain distinguishable.

## Enforcement integration

TrustForge should publish a status/check result with structured evidence.

Repositories may make that check required through normal branch protection or merge policy.

TrustForge v0 does not automatically approve, merge, or close pull requests.
