# Threat Model

## Assets to protect

- maintainer and forge credentials;
- CI/CD credentials;
- repository write access;
- release/signing keys;
- host filesystem/network;
- other workloads sharing infrastructure;
- integrity of trusted project policy;
- integrity/provenance of evidence;
- maintainer attention.

## Adversary-controlled inputs

Assume an attacker may control:

- PR source/diff;
- tests;
- build scripts;
- package manifests/lockfiles;
- generated files;
- dependency URLs and lifecycle hooks;
- issue/PR text consumed by reasoning providers;
- repository content reachable from the untrusted revision;
- provider configuration proposed inside the PR;
- workload artifacts derived from the PR.

## Threats

### T1 — Credential theft
Contribution code attempts to read environment variables, config files, sockets, metadata endpoints, or CI secrets.

**Controls:** no ambient credentials, least privilege, isolation provider, restricted filesystem/network.

### T2 — Data exfiltration
Contribution code attempts outbound communication to an attacker-controlled service.

**Controls:** default-deny/tightly scoped egress, runtime policy, event collection.

### T3 — Host/sandbox escape
Contribution code attempts privileged syscalls, filesystem escape, raw sockets, or privilege escalation.

**Controls:** replaceable isolation provider with explicit capability contract; fail closed when controls cannot be established.

### T4 — Malicious dependency behavior
A new/compromised dependency executes during install/build/test.

**Controls:** dependency evidence providers, provenance when available, isolated install/build, restricted egress.

### T5 — Policy bypass
A PR modifies policy/workflows used to validate itself.

**Controls:** effective policy loaded from trusted base/control plane; PR policy changes are evaluated as data.

### T6 — Prompt injection / reviewer manipulation
PR text/repository files attempt to influence a reasoning provider to ignore rules, leak data, or approve.

**Controls:** repository text is untrusted data; model has no merge authority; deterministic evidence/policy separated.

### T7 — Evidence tampering
Contribution-controlled code attempts to forge scanner/test results.

**Controls:** provider provenance; trusted orchestrator binds evidence to provider identity, exact revision, and raw artifact.

### T8 — Provider spoofing
A malicious adapter/result claims to be a trusted provider.

**Controls:** provider identity/configuration comes from trusted control plane; provider result provenance is authenticated/traceable.

### T9 — Provider unavailability confusion
A provider timeout/outage is interpreted as a clean result or as proof the PR is malicious.

**Controls:** explicit provider-status semantics; policy decides treatment of missing evidence.

### T10 — Attention exhaustion
Low-quality/noisy contributions consume maintainers and CI.

**Controls:** cheap triage before expensive providers; provider selection; concise normalized evidence.

### T11 — Architecture/dependency bypass
A technically functional change bypasses approved abstractions or expected subsystem boundaries.

**Controls:** deterministic rule when codified; otherwise evidence-backed `NEEDS_REVIEW`.

### T12 — Isolation implementation trust gap
The chosen sandbox runtime has its own trust boundaries and vulnerabilities.

**Controls:** isolation provider is replaceable; runtime identity/version recorded; provider-specific known limitations documented; first OpenShell POC uses trusted base environment and avoids arbitrary PR-supplied workload images while relevant upstream issue #2750 remains open.

## OpenShell POC note

As of 2026-09-04, NVIDIA/OpenShell issue #2750 documents a trust-boundary concern where privileged supervisor setup can execute helper code from an arbitrary workload image before full workload sandboxing.

The first TrustForge POC therefore:

- uses a trusted OpenShell/TrustForge base environment;
- establishes isolation before checking out/executing the PR;
- does not accept arbitrary PR-supplied workload images as the sandbox root.

Reference:
https://github.com/NVIDIA/OpenShell/issues/2750

## POC focus

The first composition/adversarial POC should demonstrate:

1. attempted credential access does not reveal host secrets;
2. unauthorized egress is blocked and recorded;
3. contribution-controlled execution happens only through isolation;
4. unapproved dependency/source can trigger deterministic policy failure;
5. subjective architecture/scope finding produces `NEEDS_REVIEW`;
6. PR cannot weaken effective policy for its own run;
7. evidence from multiple independent providers is normalized without losing provenance;
8. provider failure remains distinguishable from contribution failure.
