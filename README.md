# TrustForge

**Open infrastructure for scalable, evidence-based software contribution review.**

> Make maintainer judgment scalable — not automated away.

## Why TrustForge?

Open source development is entering a new phase: the ability to produce software changes is scaling faster than the community's ability to understand, validate, review, and safely accept them.

GitHub reported **518.7 million merged pull requests in 2025, up 29% year over year**, while comments on issues and pull requests were essentially flat at **+0.35%**. In 2026, GitHub introduced pull-request limits specifically to help maintainers manage increasing contribution volume, low-quality noise, and review overhead.

The challenge is not simply generating or reviewing code faster.

The challenge is **establishing trust at scale**.

TrustForge explores an open, maintainer-first approach to turn incoming software changes into structured **review evidence**:

```text
Contribution
    ↓
Context
    ↓
Triage
    ↓
Validation
    ↓
Risk Analysis
    ↓
Trust Evidence
    ↓
Maintainer Decision
```

TrustForge does **not** decide what gets merged.

It helps maintainers get to a well-informed decision faster.

## Core principles

1. **Maintainers stay in control.**
2. **Evidence before judgment.**
3. **Deterministic checks before probabilistic reasoning.**
4. **Explain every signal.**
5. **Project policy is authoritative.**
6. **Security is a first-class review dimension.**
7. **No universal "trust score."**
8. **Reduce noise before adding more review output.**
9. **Open standards and interoperable integrations.**
10. **Measure whether we actually save maintainer time.**

See [docs/principles.md](docs/principles.md).

## Initial scope

TrustForge v0 focuses on five capabilities:

### 1. Context
Build a compact, traceable picture of a contribution:
- what changed;
- why it changed;
- related issues and pull requests;
- affected subsystems;
- relevant code owners and maintainers;
- related historical changes.

### 2. Triage
Help identify:
- duplicates;
- missing context;
- unusually broad changes;
- likely subsystem ownership;
- changes needing specialist review.

### 3. Validation
Collect deterministic evidence from:
- CI;
- tests;
- linters;
- static analysis;
- dependency checks;
- policy checks;
- provenance/signing systems.

### 4. Risk analysis
Surface review-relevant risk:
- security-sensitive code;
- authentication/authorization changes;
- dependency or build-system changes;
- privileged operations;
- unexpected generated/binary changes;
- unusually large blast radius.

### 5. Trust evidence
Present the evidence in a concise, auditable form for a human reviewer.

## Explicitly out of scope for v0

TrustForge will **not**:
- automatically approve or merge pull requests;
- produce an opaque universal trust score;
- replace CODEOWNERS or project governance;
- treat an LLM opinion as security evidence;
- bypass required human review;
- infer contributor trustworthiness from demographic or personal attributes.

## Example evidence object

```yaml
contribution:
  id: PR-1234
  subsystem: authentication
  change_size: medium

context:
  related_issues: 3
  related_changes: 2
  codeowners_resolved: true

validation:
  unit_tests: passed
  integration_tests: passed
  static_analysis: passed

risk:
  security_sensitive_files: true
  authentication_logic_change: true
  dependency_change: false

review:
  required_expertise:
    - auth-subsystem
    - security

evidence:
  test_coverage_delta: "+4.2%"
  suspicious_patterns: none_detected
  unresolved_findings: 1
```

## Repository layout

```text
trustforge/
├── README.md
├── ROADMAP.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── LICENSE
│
├── docs/
│   ├── problem-statement.md
│   ├── vision.md
│   ├── principles.md
│   ├── architecture.md
│   ├── trust-model.md
│   └── use-cases/
│
├── research/
│   ├── ecosystem-evidence.md
│   └── references.md
│
├── specs/
│   ├── contribution-context.md
│   ├── review-evidence.md
│   └── risk-signals.md
│
├── prototypes/
├── examples/
└── .github/
    ├── ISSUE_TEMPLATE/
    └── workflows/
```

## Project status

**Exploration / pre-alpha.**

The first milestone is not to build a large AI review system. It is to validate the problem, define an interoperable review-evidence model, and test whether that model measurably reduces maintainer effort.

## Evidence behind the problem

Primary GitHub sources:

- GitHub Octoverse 2025: https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/
- GitHub, "How pull request limits are cutting down the noise" (June 18, 2026): https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/
- GitHub, "Inside the Advisory Database and what happens when vulnerability volume breaks records" (June 29, 2026): https://github.blog/security/supply-chain-security/inside-the-advisory-database-and-what-happens-when-vulnerability-volume-breaks-records/

See [research/ecosystem-evidence.md](research/ecosystem-evidence.md) for the evidence baseline.

## Contributing

TrustForge is intended to be shaped with maintainers, not merely built for them.

Start with [CONTRIBUTING.md](CONTRIBUTING.md). Early contributions are especially welcome around:
- maintainer workflows;
- review bottleneck datasets;
- evidence schemas;
- integrations with existing CI/security tools;
- evaluation methodology.

## License

Apache License 2.0.
