# Spec: Isolation Provider

Status: **draft** (`v0alpha1`)

## Purpose

Define the replaceable trust boundary used whenever TrustForge executes contributor-controlled behavior.

OpenShell is the first reference provider, but the contract is runtime-agnostic.

## Required capabilities

```text
capabilities()
create(subject, policy)
execute(command, constraints)
collect_runtime_events()
collect_artifacts()
destroy()
```

## Provider descriptor

```yaml
isolation_provider:
  id: openshell
  version: "<runtime-version>"
  adapter_version: "<adapter-version>"

capabilities:
  non_root: true
  filesystem_restrictions: true
  network_policy: true
  process_restrictions: true
  runtime_events: true
```

## Subject binding

An isolated environment MUST be bound to:

- repository;
- exact base revision;
- exact head revision;
- trusted policy revision.

## Minimum guarantees

An acceptable TrustForge isolation provider MUST:

1. prevent ambient host credentials from entering the workload by default;
2. provide a restricted execution identity;
3. restrict filesystem access according to policy;
4. restrict or explicitly govern network egress;
5. expose execution status;
6. expose relevant policy-denial/runtime events;
7. fail closed when requested controls cannot be established;
8. destroy/dispose the evaluation environment after use.

## No silent fallback

If TrustForge requires isolation and the provider cannot create a compliant environment:

```text
DO NOT:
run on host

DO:
return isolation_unavailable
apply trusted project policy
```

## Evidence output

```yaml
isolation:
  provider: openshell
  provider_version: "<version>"
  policy_revision: "<revision>"
  non_root: true
  network_default: deny
  credentials_exposed: false
  runtime_event_refs:
    - "<artifact-ref>"
```

## OpenShell POC constraint

For the first OpenShell reference POC, TrustForge uses a trusted base environment and checks out the untrusted PR **after** the isolation boundary is established.

See [../docs/isolation-model.md](../docs/isolation-model.md) for the current OpenShell trust-boundary rationale.
