# Vision

> Make maintainer judgment scalable — not automated away.

TrustForge is open infrastructure for scalable, evidence-based software contribution review.

## Desired end state

For a given contribution, a maintainer can quickly see:

1. **Context** — what changed, why, related history, owners
2. **Triage** — duplicates, missing context, breadth, specialist needs
3. **Validation** — deterministic results from CI, tests, linters, policy, provenance
4. **Risk analysis** — security-sensitive and high blast-radius signals, explained
5. **Trust evidence** — a concise, auditable package that supports a human decision

TrustForge does **not** decide what gets merged. It helps maintainers get to a well-informed decision faster.

## Design posture

- Maintainer-first and policy-aware
- Deterministic checks before probabilistic reasoning
- Explain every signal
- No universal trust score
- Interoperable with existing CI, CODEOWNERS, and security tooling
- Success measured by whether maintainer time is actually saved

## Horizon

Near term: validate the problem, define an interoperable review-evidence model, and test thin prototypes.

Longer term: become a shared layer that forges, bots, and review tools can emit and consume — without replacing project governance.
