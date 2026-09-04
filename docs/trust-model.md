# Trust model (draft)

## What "trust" means here

In TrustForge, trust is **not** a score assigned to a person. It is the degree to which a maintainer can rely on **structured, explainable evidence** about a specific contribution when deciding whether to accept it.

## Trust boundaries

| Boundary | Rule |
| --- | --- |
| Human judgment | Final accept/reject remains with maintainers and project policy |
| Deterministic evidence | CI/tests/policy/provenance are first-class |
| Probabilistic aids | May organize or narrate; never substitute for security validation |
| Project policy | CODEOWNERS, required checks, and governance override defaults |
| Contributor identity | May be used for routing/contact; never for demographic inference |

## Evidence integrity

Review evidence should be:

- **Traceable** — each claim points to a source artifact or observation
- **Timestamped** — when evidence was collected relative to the revision
- **Scoped** — tied to a specific contribution revision (commit SHA / PR head)
- **Explainable** — signals include rationale and verification hints
- **Non-opaque** — no hidden universal score that collapses all dimensions

## Explicit non-claims

TrustForge does not claim that:

- passing checks means the change is correct or safe;
- absence of risk signals means absence of risk;
- similar historical merges imply this merge is appropriate;
- contributor history alone establishes change quality.

## Related specs

- [contribution-context.md](../specs/contribution-context.md)
- [review-evidence.md](../specs/review-evidence.md)
- [risk-signals.md](../specs/risk-signals.md)
