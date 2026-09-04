# Spec: Evidence Provider

Status: **draft** (`v0alpha1`)

## Purpose

Define how existing tools contribute evidence to TrustForge without becoming part of TrustForge core.

## Provider descriptor

```yaml
provider:
  id: osv-scanner
  category: dependency
  version: "<provider-version>"
  adapter_version: "<adapter-version>"

capabilities:
  - dependency_vulnerability

execution:
  class: isolated_required
```

## Provider categories

Recommended initial values:

- `context`
- `ci`
- `static_analysis`
- `dependency`
- `policy`
- `provenance`
- `runtime`
- `supply_chain_context`
- `reasoning`

## Execution classes

### `data_only`

The adapter does not execute contribution-controlled code.

### `isolated_required`

The provider may execute/load contribution-controlled code or trigger package/build/plugin behavior.

It MUST execute through `IsolationProvider`.

### `external_service`

The provider sends data to a remote service.

It MUST declare data-transfer requirements and be permitted by repository policy.

## Evidence envelope

```yaml
schema_version: trustforge.dev/evidence/v0alpha1

provider:
  id: semgrep
  category: static_analysis
  version: "<provider-version>"
  adapter_version: "<adapter-version>"

subject:
  repository: owner/repo
  base_revision: "<sha>"
  head_revision: "<sha>"
  contribution_id: "<forge-id>"

evidence:
  class: verified
  kind: static_analysis_finding
  result: finding
  severity: high
  summary: "<short description>"

source:
  artifact_ref: "<raw-result-reference>"
  generated_at: "<timestamp>"
  isolated: true
  isolation_provider: openshell

extensions:
  semgrep: {}
```

## Evidence classes

- `verified`
- `observed`
- `inferred`
- `human`

Provider adapters MUST NOT misclassify inference as verified evidence.

## Required properties

Every evidence item MUST identify:

1. provider ID;
2. provider/adapter version where available;
3. exact contribution revision;
4. evidence class;
5. evidence kind/result;
6. source/raw artifact reference when available;
7. execution/isolation context where relevant.

## Failure envelope

Provider execution failure is not the same as a failed contribution check.

```yaml
provider_status:
  state: provider_error
  reason: "<reason>"
```

Suggested states:

- `ok`
- `provider_unavailable`
- `provider_error`
- `provider_timeout`
- `not_applicable`
- `inconclusive`

Trusted project policy decides whether missing evidence produces `PASS`, `NEEDS_REVIEW`, or `POLICY_FAILED`.

## Hard-decision rule

An evidence provider MUST NOT directly approve or merge a contribution.

A provider may report deterministic failure evidence.

TrustForge maps trusted policy + evidence to the final TrustForge policy outcome.

## Extensions

Provider-specific details SHOULD live under a namespaced extension object.

The portable core schema should only absorb a provider-specific field when multiple providers share the concept and maintainers benefit from standardization.
