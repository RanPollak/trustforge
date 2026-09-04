# Threat Model

## Assets to protect

- maintainer credentials and tokens;
- CI/CD credentials;
- repository write access;
- release/signing keys;
- host filesystem and network;
- other workloads sharing infrastructure;
- integrity of TrustForge policy and evidence;
- maintainer attention.

## Adversary-controlled inputs

Assume an attacker may control:

- PR source code and diff;
- test code;
- build scripts;
- package manifests and lockfiles;
- generated files;
- dependency URLs and package lifecycle hooks;
- issue/PR text consumed by models;
- repository content reachable from the untrusted revision.

## Threats

### T1 — Credential theft
Contribution code attempts to read environment variables, config files, sockets, metadata endpoints, or injected CI secrets.

**Controls:** no ambient credentials, least privilege, restricted filesystem/network, isolated runtime.

### T2 — Data exfiltration
Contribution code attempts outbound communication to an attacker-controlled service.

**Controls:** default-deny/tightly scoped egress, destination policy, policy logs.

### T3 — Host or sandbox escape
Contribution code attempts privileged syscalls, raw sockets, filesystem escape, or privilege escalation.

**Controls:** non-root execution, capability reduction, seccomp, filesystem restrictions, disposable sandbox.

### T4 — Malicious dependency behavior
A newly introduced or compromised dependency executes during install/build/test.

**Controls:** dependency-source policy, provenance where available, isolated install/build, restricted egress.

### T5 — Policy bypass
A PR modifies the policy or workflow used to validate itself.

**Controls:** policy is loaded from the trusted base revision/control plane; changes to policy files are evaluated as data, not applied to the current run.

### T6 — Prompt injection / reviewer manipulation
PR text or repository files attempt to influence an AI-assisted reviewer to ignore rules, leak data, or approve the PR.

**Controls:** treat repository text as untrusted data, tool-level authorization, deterministic policy separation, no model authority to merge.

### T7 — Evidence tampering
Contribution-controlled code attempts to forge validation output.

**Controls:** evidence collection owned by the trusted orchestrator, bind evidence to source revision and tool identity, preserve raw artifacts/logs.

### T8 — Attention exhaustion
Low-quality or deliberately noisy contributions consume maintainer and CI capacity.

**Controls:** pre-execution triage, rate/queue policy, cheap checks before expensive checks, concise evidence.

### T9 — Architecture or dependency bypass
A contribution introduces a technically functional library or implementation path that bypasses the project's approved abstraction, dependency policy, or subsystem ownership.

**Controls:** explicit architecture/dependency rules where possible; otherwise evidence-backed `NEEDS_REVIEW` routed to appropriate maintainers.

## POC threat-model focus

The first POC should explicitly demonstrate:

1. attempted credential/environment access does not reveal host secrets;
2. unauthorized egress is blocked and logged;
3. contribution-controlled build/test code executes only inside OpenShell;
4. an unapproved dependency/source can trigger policy failure;
5. a subjective architecture/scope finding produces `NEEDS_REVIEW`, not automatic rejection;
6. a PR cannot weaken the trusted policy used to evaluate itself.
