# Isolation Model

## Why isolation is a first-class TrustForge concern

A pull request is not just a diff to read. It may contain or influence code that executes during:

- dependency installation;
- package lifecycle hooks;
- build steps;
- test execution;
- code generation;
- linters or plugins;
- custom repository scripts.

Static analysis cannot guarantee that all malicious behavior will be detected before execution. TrustForge therefore treats contributor-controlled execution as **untrusted by default**.

> **Never trust a contribution enough to execute it before you have isolated it.**

## Trust boundary

```text
Trusted control plane
  - trusted base policy
  - provider orchestration
  - evidence normalization
  - forge credentials
            │
            ▼
     IsolationProvider
            │
            ▼
┌─────────────────────────────────┐
│ Isolated evaluation environment │
│                                 │
│ exact PR revision               │
│ install / build / test / scan   │
│ no ambient host credentials     │
│ restricted filesystem           │
│ non-root workload               │
│ controlled network egress       │
└─────────────────────────────────┘
            │
            ▼
Auditable runtime/provider evidence
```

The contribution must not be able to modify the effective policy used to evaluate itself.

## Runtime-agnostic contract

TrustForge core should not embed OpenShell-specific syntax.

An `IsolationProvider` exposes a portable contract such as:

```text
capabilities()
create(revision, policy)
execute(command)
collect_artifacts()
collect_runtime_events()
destroy()
```

See [../specs/isolation-provider.md](../specs/isolation-provider.md).

## OpenShell reference runtime

The first POC uses **NVIDIA OpenShell** as the reference isolation provider.

OpenShell documents overlapping sandbox controls including:

- restricted/unprivileged workload execution;
- Landlock filesystem restrictions;
- seccomp restrictions;
- network namespace routing;
- policy-controlled outbound access;
- logged policy denials.

Official references:

- https://github.com/NVIDIA/OpenShell
- https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md
- https://docs.nvidia.com/openshell/sandboxes/policies
- https://github.com/NVIDIA/OpenShell/tree/main/examples/sandbox-policy-quickstart

## Important POC constraint: trusted base image

As of **2026-09-04**, OpenShell issue [#2750](https://github.com/NVIDIA/OpenShell/issues/2750) is open and documents a trust-boundary concern for **arbitrary untrusted workload images**: privileged supervisor setup can execute helper code supplied by the workload image before the workload is fully sandboxed.

TrustForge should therefore make the first POC deliberately narrower:

```text
Trusted TrustForge/OpenShell base environment
                ↓
Isolation boundary established
                ↓
Privilege/network/filesystem controls established
                ↓
Checkout exact untrusted PR revision
                ↓
Install / build / test / scan
```

The first POC SHOULD NOT accept a PR-supplied arbitrary OCI/workload image as the isolation root.

This constraint should be revisited when the OpenShell trust-boundary behavior changes and the relevant upstream work is complete.

## Minimum POC guarantees

1. PR-controlled build/test code executes only behind `IsolationProvider`.
2. Host credentials are not available to the contribution.
3. Unauthorized outbound network access is denied and observable.
4. The contribution cannot weaken trusted policy for its own evaluation.
5. Isolation failure fails closed.
6. Runtime denials become evidence, not automatic proof of malicious intent.
7. The isolation provider identity/version is attached to evidence.
8. The first OpenShell POC uses a trusted base environment rather than an arbitrary PR-supplied workload image.

## What isolation does not prove

Isolation does **not** mean the contribution is safe or correct.

Separate evidence is still required for:

- architectural correctness;
- logical correctness;
- dependency/provenance trust;
- security vulnerabilities;
- project scope and necessity;
- maintainer governance.
