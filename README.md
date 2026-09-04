# TrustForge

**Open review-evidence orchestration for software contributions.**

> **Make maintainer judgment scalable — not automated away.**

TrustForge is **not another AI code reviewer, scanner, policy engine, or sandbox**.

It is an open layer that brings evidence from existing tools together so maintainers can make faster, better-supported trust decisions about incoming software contributions.

## The problem

Open source development is entering a phase where the ability to produce software changes is scaling faster than the community's ability to understand, validate, review, and safely accept them.

GitHub reported **518.7 million merged pull requests in 2025, up 29% year over year**, while comments on issues and pull requests were essentially flat at **+0.35%**. In 2026, GitHub introduced pull-request limits specifically to help maintainers manage increasing contribution volume, low-quality noise, and review overhead.

The challenge is not simply generating or reviewing code faster.

The challenge is **establishing trust at scale**.

## What TrustForge owns

TrustForge intentionally keeps its core small.

```text
Untrusted Contribution
        │
        ▼
Contribution Context
        │
        ▼
Provider Orchestration
        │
        ├── CI / tests
        ├── static analysis
        ├── dependency intelligence
        ├── provenance / attestations
        ├── project policy
        ├── isolated runtime evidence
        └── optional reasoning/context providers
        │
        ▼
Normalized Review Evidence
        │
        ▼
Policy Decision
PASS | NEEDS_REVIEW | POLICY_FAILED
        │
        ▼
Maintainer Decision
```

The TrustForge core is responsible for:

1. **Contribution context** — bind evidence to the exact change being reviewed.
2. **Provider contracts** — let existing tools contribute evidence without becoming TrustForge-specific.
3. **Isolation contracts** — ensure contributor-controlled execution happens behind a replaceable isolation boundary.
4. **Evidence normalization** — distinguish verified, observed, inferred, and human evidence.
5. **Policy-decision semantics** — map explicit project policy to explainable outcomes.
6. **Orchestration** — run the right evidence providers for the contribution and project policy.
7. **Maintainer presentation** — produce a concise, auditable evidence package.
8. **Evaluation** — measure whether TrustForge actually reduces maintainer effort and noise.

## What TrustForge does not rebuild

Mature open-source tools already solve important parts of this problem.

TrustForge should integrate them rather than compete with them.

| Need | Existing ecosystem examples | TrustForge role |
| --- | --- | --- |
| AI-assisted PR understanding | PR-Agent and similar tools | Optional reasoning/context provider |
| PR/repository rules | Danger, OPA/Conftest | Consume policy results |
| Static analysis | Semgrep and project-native analyzers | Normalize findings as evidence |
| Dependency vulnerability analysis | OSV-Scanner, GitHub Dependency Review | Normalize dependency evidence |
| OSS dependency/compliance orchestration | OSS Review Toolkit (ORT) | Consume relevant dependency/compliance evidence |
| Project security posture | OpenSSF Scorecard | Consume posture signals where relevant |
| Provenance / attestations | Sigstore, in-toto | Verify and attach provenance evidence |
| Supply-chain metadata aggregation | GUAC | Consume relationship/context evidence |
| Runtime isolation | OpenShell | Isolation provider |
| CI runtime observation | Harden-Runner and similar tooling | Runtime evidence provider |
| CI / test execution | Existing project CI | Reuse existing results |

See [docs/ecosystem-map.md](docs/ecosystem-map.md).

## Why TrustForge is different

Most tools answer one question:

- *Does the code contain a known security pattern?*
- *Did the tests pass?*
- *Is this dependency vulnerable?*
- *Did policy X pass?*
- *Can this workload be isolated?*
- *What does an AI reviewer think about the diff?*

A maintainer still has to combine those answers into a decision.

TrustForge focuses on that missing layer:

> **What evidence do we have about this exact contribution, where did it come from, how trustworthy is each signal, which project policies apply, what remains uncertain, and what deserves human attention?**

The output is not a universal trust score.

It is a **review-evidence package**.

## Evidence classes

TrustForge keeps evidence classes explicit:

- **Verified** — deterministic/reproducible results such as CI, tests, signatures, policy-engine output, or scanner findings with provenance.
- **Observed** — repository facts such as changed files, owners, dependency diffs, or runtime events.
- **Inferred** — reasoned findings such as likely architectural mismatch, scope expansion, or blast radius.
- **Human** — maintainer/reviewer decisions and attestations.

An inferred signal is never presented as verified evidence.

## Isolation is a first-class boundary

A pull request may execute code during dependency installation, build, tests, package hooks, code generation, or project-specific tooling.

TrustForge therefore follows this principle:

> **Never execute untrusted contribution code without isolation.**

**OpenShell is the reference isolation runtime for the first TrustForge POC**, but the TrustForge evidence model is runtime-agnostic through an `IsolationProvider` contract.

See [docs/isolation-model.md](docs/isolation-model.md).

## Policy outcomes

TrustForge separates hard project policy from contextual judgment:

- **`PASS`** — required deterministic project checks passed.
- **`NEEDS_REVIEW`** — no hard violation was proven, but architecture, scope, security, or context requires human judgment.
- **`POLICY_FAILED`** — an explicit machine-evaluable project policy failed.

An LLM or probabilistic finding alone cannot produce `POLICY_FAILED`.

TrustForge does not automatically approve, merge, or close a pull request.

## First vertical-slice POC

The first POC should prove **composition**, not reimplementation.

Candidate provider stack:

```text
GitHub PR
   │
   ├── repository context / CODEOWNERS
   ├── existing CI / tests
   ├── Semgrep                → static-analysis evidence
   ├── OSV-Scanner            → dependency-vulnerability evidence
   ├── OPA / Conftest         → deterministic policy evidence
   ├── OpenShell              → isolated execution + runtime evidence
   └── provenance provider    → Sigstore / in-toto when available
               │
               ▼
        TrustForge Core
               │
               ▼
      Normalized Evidence
               │
               ▼
PASS | NEEDS_REVIEW | POLICY_FAILED
               │
               ▼
        Maintainer Report
```

These are **candidate/reference integrations**, not claimed implemented support.

See [prototypes/openshell-pr-evaluator/README.md](prototypes/openshell-pr-evaluator/README.md).

## Core principles

1. **Maintainers stay in control.**
2. **Evidence before judgment.**
3. **Deterministic checks before probabilistic reasoning.**
4. **Explain every signal.**
5. **Project policy is authoritative.**
6. **Security is a first-class review dimension.**
7. **No universal trust score.**
8. **Reduce noise before adding more review output.**
9. **Open standards and interoperable integrations.**
10. **Measure whether we actually save maintainer time.**
11. **Never execute untrusted contribution code without isolation.**
12. **Integrate before inventing.**

See [docs/principles.md](docs/principles.md).

## Repository layout

```text
trustforge/
├── README.md
├── ROADMAP.md
├── CONTRIBUTING.md
├── SECURITY.md
│
├── docs/
│   ├── problem-statement.md
│   ├── vision.md
│   ├── principles.md
│   ├── architecture.md
│   ├── trust-model.md
│   ├── isolation-model.md
│   ├── policy-model.md
│   ├── threat-model.md
│   ├── ecosystem-map.md
│   └── integration-strategy.md
│
├── specs/
│   ├── contribution-context.md
│   ├── review-evidence.md
│   ├── risk-signals.md
│   ├── policy-decision.md
│   ├── evidence-provider.md
│   └── isolation-provider.md
│
├── providers/
│   └── README.md
│
├── prototypes/
│   └── openshell-pr-evaluator/
│
├── examples/
└── research/
```

## Project status

**Exploration / pre-alpha.**

The immediate goal is not to build a large AI review system. It is to:

1. validate the maintainer problem;
2. define interoperable evidence/provider contracts;
3. compose existing open-source tools into one vertical slice;
4. prove isolated evaluation of untrusted PR behavior;
5. measure whether the resulting evidence package reduces maintainer effort.

## Evidence behind the problem

Primary GitHub sources:

- GitHub Octoverse 2025: https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/
- GitHub, "How pull request limits are cutting down the noise" (June 18, 2026): https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/
- GitHub, "Inside the Advisory Database and what happens when vulnerability volume breaks records" (June 29, 2026): https://github.blog/security/supply-chain-security/inside-the-advisory-database-and-what-happens-when-vulnerability-volume-breaks-records/

See [research/ecosystem-evidence.md](research/ecosystem-evidence.md) and [research/references.md](research/references.md).

## Contributing

TrustForge is intended to be shaped **with maintainers and existing open-source projects**, not built as a replacement for them.

Before proposing a new scanner, sandbox, policy language, provenance system, or AI reviewer, check [docs/ecosystem-map.md](docs/ecosystem-map.md) and ask whether the capability should instead be a provider integration.

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

Apache License 2.0.
