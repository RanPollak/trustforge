# Spec: Contribution context

Status: **draft** (v0)

## Purpose

Describe a compact, traceable picture of a contribution for downstream triage, validation, risk analysis, and trust evidence.

## Conceptual fields

| Field | Description |
| --- | --- |
| `contribution.id` | Forge-native identifier (e.g. `PR-1234`) |
| `contribution.revision` | Commit SHA / head revision evidence was collected for |
| `contribution.subsystem` | Primary subsystem label if known |
| `contribution.change_size` | Qualitative or quantitative size (`small` / `medium` / `large` or LOC metrics) |
| `context.related_issues` | Linked or inferred related issues (count and/or IDs) |
| `context.related_changes` | Related historical PRs/commits |
| `context.codeowners_resolved` | Whether CODEOWNERS (or equivalent) resolved for touched paths |
| `context.owners` | Resolved maintainers / teams |
| `context.rationale` | Stated why (from description/commits), if available |
| `context.files_touched` | Optional summary of paths / packages |

## Requirements

- Context MUST be scoped to a specific revision.
- Context SHOULD cite sources for related issues/changes when claimed.
- Context MUST NOT encode demographic or personal-attribute inferences about contributors.

## Example

See [../examples/evidence-pr-1234.yaml](../examples/evidence-pr-1234.yaml) (`contribution` and `context` sections).
