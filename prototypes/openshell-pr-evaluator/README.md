# OpenShell PR Evaluator POC

## Goal

Demonstrate TrustForge's first end-to-end security workflow:

> Take an untrusted pull request, evaluate it inside an isolated OpenShell sandbox under repository-defined policy, and return structured trust evidence to the maintainer.

## Why this POC

A PR can influence executable behavior through source code, tests, build scripts, package hooks, generated code, and new dependencies. Static review alone cannot guarantee that every malicious behavior is detected before execution.

Isolation therefore becomes part of the review architecture, not an implementation detail.

## POC flow

```text
GitHub PR
   ↓
Trusted orchestrator reads base policy
   ↓
Context + cheap static/precheck policy
   ↓
Create OpenShell sandbox
   ↓
Checkout exact PR revision
   ↓
Install / Build / Test / Scan
   ↓
Collect OpenShell policy-denial/runtime events
   ↓
Analyze dependencies + architectural fit
   ↓
TrustForge Policy Decision
   ↓
PASS / NEEDS_REVIEW / POLICY_FAILED
   ↓
Structured PR check + evidence report
```

## Security properties to demonstrate

1. PR-controlled code runs only in the sandbox.
2. The child workload is unprivileged/restricted by the isolation runtime.
3. Host credentials are not available to the PR.
4. Egress is denied by default except explicitly approved destinations.
5. Unexpected network attempts are blocked and surfaced as evidence.
6. Dependency source policy is enforced.
7. A hard failure comes only from an explicit deterministic repository rule.
8. Architectural/scope reasoning can request maintainer review but cannot independently reject the PR.
9. Policy is loaded from a trusted base/control plane; the PR cannot weaken the policy used for its own run.

## Demo attack cases

### A. Environment/credential probe
A malicious test attempts to enumerate environment variables and known credential paths.

Expected result: no host secret is available; access denials are recorded where applicable.

### B. Data-exfiltration attempt
A test or package hook attempts to send data to an unauthorized destination.

Expected result: OpenShell egress policy denies the connection; TrustForge records the denial.

### C. Untrusted dependency source
The PR adds a dependency from a registry/source not allowed by repository policy.

Expected result: `POLICY_FAILED`.

### D. Wrong architectural library
The PR introduces a technically valid library that bypasses the project's preferred abstraction.

If no deterministic project policy codifies the requirement, expected result: `NEEDS_REVIEW` with context and suggested reviewer.

### E. Scope expansion
The PR changes unrelated files outside the issue's expected subsystem.

Expected result: `NEEDS_REVIEW` unless the repository has an explicit path/scope policy that makes it a hard failure.

### F. Self-weakening policy change
The PR edits TrustForge policy or validation configuration in a way that would permit its own behavior.

Expected result: current evaluation continues using trusted base policy; the proposed policy change is reviewed as contribution data.

## Implementation boundary

This POC integrates OpenShell through an isolation adapter. TrustForge should not make OpenShell-specific policy syntax its core public evidence schema.

## OpenShell references

- https://github.com/NVIDIA/OpenShell
- https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md
- https://docs.nvidia.com/openshell/sandboxes/policies
- https://github.com/NVIDIA/OpenShell/tree/main/examples/sandbox-policy-quickstart
