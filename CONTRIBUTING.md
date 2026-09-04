# Contributing to TrustForge

Thanks for helping shape TrustForge. This project is intended to be built **with** maintainers, not merely for them.

## Ways to contribute

Early contributions that help most:

- **Maintainer workflows** — how you triage, review, and decide today; where time is lost
- **Review bottleneck datasets** — anonymized or public examples of hard reviews (with permission)
- **Evidence schemas** — fields, semantics, and mappings under `specs/`
- **Integrations** — adapters for existing CI, security, and policy tools
- **Evaluation methodology** — how we measure whether TrustForge saves maintainer time

## Ground rules

1. Read [docs/principles.md](docs/principles.md) before proposing features that automate judgment.
2. Prefer deterministic, explainable signals over opaque scores.
3. Do not propose automatic merge/approve in v0 discussions without a clear opt-in governance story — and even then, treat it as out of scope until evaluation says otherwise.
4. Never infer contributor trustworthiness from demographic or personal attributes.
5. Be respectful; follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Process

1. Open an issue describing the problem or proposal before large PRs.
2. Keep changes focused; separate schema, docs, and prototype work when practical.
3. For schema changes, include an updated example under `examples/`.
4. For security-sensitive reports, use [SECURITY.md](SECURITY.md) instead of a public issue.

## Development notes

This repository is primarily documentation, specs, research, and thin prototypes. There is no required application runtime for contributing to docs or schemas.

Suggested local checks before opening a PR:

- Links in Markdown resolve
- Spec examples remain consistent with the documented fields
- New files fit the [repository layout](README.md#repository-layout)

## License

By contributing, you agree that your contributions are licensed under the [Apache License 2.0](LICENSE).
