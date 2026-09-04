# Use cases

## UC1 — Maintainer triage of a high-volume inbox

A maintainer opens a pull request and needs to know, in under a minute:

- whether context is missing;
- whether the change is a likely duplicate;
- which subsystem and owners are implicated;
- whether specialist (e.g. security) review is warranted.

TrustForge provides triage hints backed by context and risk signals — not an auto-close decision.

## UC2 — Security-sensitive authentication change

A PR touches authentication logic. The maintainer needs:

- clear marking of security-sensitive files and auth-related diffs;
- deterministic validation status (tests, static analysis, policy);
- unresolved findings listed explicitly;
- required expertise tags for routing.

TrustForge does not approve the PR; it packages evidence for the security-aware review.

## UC3 — Dependency / build-system change

A contribution updates lockfiles or CI/build definitions. Risk analysis surfaces dependency and build-system change flags, validation includes dependency checks, and the evidence package highlights blast radius for human review.

## UC4 — New contributor with incomplete issue linkage

Context assembly shows related issues unresolved or missing. Triage flags missing context so maintainers can request linkage before deep review time is spent.

## UC5 — Evaluation of whether evidence saves time

A project runs an evaluation comparing time-to-first-meaningful-review with and without the evidence package, feeding results into `research/` methodology — aligning with the principle that TrustForge must measure real maintainer-time savings.
