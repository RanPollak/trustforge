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

TrustForge separates the trusted orchestrator from the untrusted evaluation environment.

```text
Trusted control plane
  - base repository policy
  - orchestration
  - evidence collection
  - repository credentials
            │
            ▼
┌─────────────────────────────────┐
│ Isolated evaluation environment │
│                                 │
│ exact PR revision               │
│ install / build / test / scan   │
│ no ambient host credentials     │
│ restricted filesystem           │
│ non-root contribution process   │
│ controlled network egress       │
└─────────────────────────────────┘
            │
            ▼
Auditable runtime evidence
```

The PR must not be able to modify the policy used to evaluate itself. The trusted base/control plane selects the effective policy.

## OpenShell reference runtime

The first TrustForge POC uses **NVIDIA OpenShell** as the reference isolation runtime.

OpenShell documents overlapping sandbox controls including:

- a restricted/unprivileged child process for agent/user code;
- Landlock filesystem restrictions;
- seccomp restrictions on dangerous syscalls;
- network namespace routing;
- a policy proxy that evaluates outbound access;
- structured/logged policy denials.

OpenShell's network policy model supports deny-by-default behavior and explicit host/port/binary rules, with deeper HTTP method/path enforcement for configured REST endpoints.

Official references:
- https://github.com/NVIDIA/OpenShell
- https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md
- https://docs.nvidia.com/openshell/sandboxes/policies
- https://github.com/NVIDIA/OpenShell/tree/main/examples/sandbox-policy-quickstart

## TrustForge runtime contract

TrustForge should not make OpenShell-specific syntax part of its core public schema. Instead, an `IsolationRuntime` adapter should provide capabilities such as:

```text
create(revision, policy)
execute(command)
collect_artifacts()
collect_runtime_events()
destroy()
```

The runtime should return normalized evidence such as:

- process privilege level;
- filesystem policy applied;
- network policy applied;
- attempted/denied egress;
- execution exit status;
- raw log/artifact references;
- runtime identity/version.

## Minimum POC guarantees

The first POC should demonstrate that:

1. PR-controlled build/test code executes only in isolation.
2. Host credentials are not available to the PR.
3. Unauthorized outbound network access is denied and observable.
4. The contribution cannot weaken the trusted policy used for its own evaluation.
5. Isolation failures fail closed rather than silently running on the host.
6. Runtime denials become evidence, not automatic proof of malicious intent.

## What isolation does not prove

Isolation does **not** mean the contribution is safe or correct.

A sandbox can limit impact while TrustForge still needs separate evidence for:

- architectural correctness;
- logical correctness;
- dependency/provenance trust;
- security vulnerabilities;
- project scope and necessity;
- maintainer governance.
