# Policy Model

TrustForge separates **project policy** from **model judgment**.

## Policy outcomes

### PASS
All required deterministic checks passed.

### NEEDS_REVIEW
No explicit hard policy violation was proven, but one or more findings require maintainer judgment.

Examples:
- a new library appears inconsistent with established architecture;
- the change may be broader than the linked issue requires;
- an unusual implementation path has no explicit project rule against it;
- a sandbox denial occurred that may be legitimate but deserves review.

### POLICY_FAILED
A machine-evaluable required project policy failed.

Examples:
- dependency source is outside the allowed registries;
- a protected path changed when policy explicitly forbids it;
- required provenance/signature is missing;
- sandbox execution attempted network access that policy explicitly marks as a hard failure;
- a required test/security gate failed.

## Hard vs. advisory policy

```text
Explicit deterministic project rule
          ↓
     Hard enforcement
          ↓
  required status check fails

Contextual / probabilistic finding
          ↓
     Advisory evidence
          ↓
      NEEDS_REVIEW
```

This distinction prevents an LLM from becoming an opaque merge authority.

## Architectural/library fit

TrustForge should assess whether a contribution uses the project's established abstractions and approved libraries.

Evidence can come from:
- repository policy;
- architecture documentation;
- dependency manifests;
- existing code patterns;
- ownership/CODEOWNERS;
- historical changes.

If a rule is explicit and machine-evaluable, it can be enforced. If the conclusion depends on interpretation, TrustForge should produce `NEEDS_REVIEW` and show the evidence that led to the finding.

## "Unnecessary change" policy

TrustForge should help detect scope expansion, unrelated refactors, duplicated functionality, and additions that do not appear necessary for the stated issue. However, necessity is often contextual.

Therefore:

- if the repository has an explicit enforceable rule, TrustForge can fail the policy;
- if necessity is inferred from architecture/history/issue context, TrustForge should surface the finding as `NEEDS_REVIEW`, with evidence.

## Trusted policy source

The policy used for the current evaluation must come from a **trusted base revision or control plane**. A PR may propose changes to policy files, but those proposed changes are evaluated as data and do not automatically apply to the same run.

This prevents a contribution from weakening the rules used to evaluate itself.

## Example policy

```yaml
version: trustforge.dev/v0alpha1

dependencies:
  allowed_registries:
    - pypi.org
    - registry.npmjs.org
  deny_unknown_sources: true

repository:
  protected_paths:
    - .github/workflows/**
  protected_path_action: needs_review

execution:
  privileged: false
  host_filesystem: false

network:
  default: deny
  allow:
    - github.com
    - pypi.org
    - registry.npmjs.org

architecture:
  advisory_rules:
    - id: preferred-auth-library
      description: Use the project's approved authentication abstraction.

security:
  unsigned_binary:
    action: fail
```

## Enforcement integration

TrustForge should publish a status/check result with structured evidence. Repositories can make that status required through their normal branch-protection or merge-policy system.

TrustForge v0 should not automatically close pull requests.
