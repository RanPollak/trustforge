# Contributing to TrustForge

Thanks for helping shape TrustForge. This project is intended to be built **with maintainers and adjacent open-source communities**, not merely for them.

## Start with the duplication question

Before proposing a substantial new capability, ask:

> **Does a mature open-source project already produce this signal or provide this execution capability?**

If yes, the default TrustForge approach is to **integrate it through a provider contract**, not reimplement it.

Read:

- [docs/principles.md](docs/principles.md)
- [docs/ecosystem-map.md](docs/ecosystem-map.md)
- [docs/integration-strategy.md](docs/integration-strategy.md)

## Ways to contribute

Early contributions that help most:

- **Maintainer workflows** — where triage/review time is actually lost
- **Evidence contracts** — normalized fields and provenance semantics
- **Provider adapters** — mappings from existing tools into TrustForge evidence
- **Isolation adapters** — safe execution runtimes behind the isolation contract
- **Evaluation datasets/methodology** — evidence that TrustForge reduces maintainer effort
- **Ecosystem research** — identifying overlap before new functionality is built

## A provider is usually better than a new subsystem

Examples:

- Need static analysis? Prefer a Semgrep/project-native analyzer adapter.
- Need vulnerability intelligence? Prefer OSV-Scanner or another established source.
- Need policy-as-code? Prefer OPA/Conftest or an existing project policy engine.
- Need provenance? Prefer Sigstore/in-toto.
- Need isolated execution? Implement `IsolationProvider`; do not build a new sandbox into core.
- Need AI-assisted PR reasoning? Treat it as an optional reasoning provider.

A proposal to build a new engine should explain why existing open-source tools cannot satisfy the requirement through an adapter.

## Ground rules

1. Maintainers stay in control.
2. Prefer deterministic, explainable signals over opaque scores.
3. Keep verified/observed/inferred/human evidence separate.
4. Never label model opinion as security validation.
5. Never execute untrusted contribution code outside an isolation boundary.
6. A PR must not be able to weaken the policy used for its own evaluation.
7. Automatic approve/merge is out of scope for v0.
8. Never infer contributor trustworthiness from demographic or personal attributes.
9. Keep provider-specific details out of the portable core schema unless they are truly cross-provider concepts.
10. Be respectful and follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Process

1. Open an issue before large changes.
2. For a new capability, include a short **existing-tools assessment**.
3. For a provider, document:
   - provider/category;
   - inputs;
   - execution class;
   - normalized outputs;
   - raw evidence/provenance;
   - failure semantics.
4. For schema changes, update an example under `examples/`.
5. For security-sensitive reports, follow [SECURITY.md](SECURITY.md).

## Development notes

This repository is primarily documentation, specs, research, and thin prototypes.

Suggested local checks before opening a PR:

- `git diff --check`
- Markdown links resolve
- Spec examples remain consistent
- New files fit the documented project boundary
- New engines have an explicit justification for why an adapter is insufficient

## License

By contributing, you agree that your contributions are licensed under the [Apache License 2.0](LICENSE).
