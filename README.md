# TrustForge

**Open infrastructure for scalable, evidence-based software contribution review.**

> Make maintainer judgment scalable — not automated away.

## Why TrustForge?

Open source development is entering a new phase: the ability to produce software changes is scaling faster than the community's ability to understand, validate, review, and safely accept them.

GitHub reported **518.7 million merged pull requests in 2025, up 29% year over year**, while comments on issues and pull requests were essentially flat at **+0.35%**. In 2026, GitHub introduced pull-request limits specifically to help maintainers manage increasing contribution volume, low-quality noise, and review overhead.

The challenge is not simply generating or reviewing code faster.

The challenge is **establishing trust at scale**.

TrustForge explores an open, maintainer-first approach that turns incoming software changes into structured **review evidence**:

```text
Contribution
    ↓
Context
    ↓
Triage
    ↓
Policy Precheck
    ↓
Isolated Evaluation
    ↓
Validation + Risk Analysis
    ↓
Policy Enforcement
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
11. **Never execute untrusted contribution code without isolation.**

See [docs/principles.md](docs/principles.md).

## Initial scope

TrustForge v0 focuses on seven capabilities.

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

### 3. Policy precheck
Evaluate repository-defined requirements before executing untrusted code:
- allowed dependency sources and registries;
- protected or forbidden paths;
- required checks and ownership;
- repository-specific architectural rules;
- scope rules that can be evaluated deterministically.

### 4. Isolated evaluation
Execute contributor-controlled behavior only inside a restricted environment with:
- non-root execution;
- restricted filesystem access;
- default-deny or tightly controlled network egress;
- no ambient host credentials;
- syscall/process restrictions;
- auditable policy-denial events.

**OpenShell is the reference isolation runtime for the first TrustForge POC.** TrustForge itself remains runtime-agnostic through an isolation adapter boundary.

### 5. Validation
Collect deterministic evidence from:
- CI;
- tests;
- linters;
- static analysis;
- dependency checks;
- policy checks;
- provenance/signing systems.

### 6. Risk analysis
Surface review-relevant risk:
- security-sensitive code;
- authentication/authorization changes;
- dependency or build-system changes;
- privileged operations;
- unexpected generated/binary changes;
- unusually large blast radius;
- suspicious runtime behavior observed inside isolation.

### 7. Trust evidence
Present the evidence in a concise, auditable form for a human reviewer.

## Policy outcomes

TrustForge distinguishes between explicit policy enforcement and contextual review guidance:

- **`PASS`** — required deterministic policy checks passed.
- **`NEEDS_REVIEW`** — no hard policy violation was proven, but architecture, scope, security, or context requires human judgment.
- **`POLICY_FAILED`** — an explicit machine-evaluable repository policy failed.

An LLM or probabilistic finding alone cannot produce `POLICY_FAILED`.

## Explicitly out of scope for v0

TrustForge will **not**:
- automatically approve or merge pull requests;
- produce an opaque universal trust score;
- replace CODEOWNERS or project governance;
- treat an LLM opinion as security evidence;
- bypass required human review;
- automatically close a PR based only on probabilistic/LLM judgment;
- execute untrusted PR code on the host or with ambient credentials;
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

isolation:
  runtime: openshell
  non_root: true
  network_default: deny
  blocked_egress_attempts: 1

validation:
  unit_tests: passed
  integration_tests: passed
  static_analysis: passed
  dependency_checks: passed

policy:
  status: NEEDS_REVIEW
  deterministic_failures: 0
  advisory_findings: 1

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

## First POC: isolated PR evaluation

The first end-to-end POC is intentionally concrete:

> Take an untrusted pull request, evaluate it inside an isolated OpenShell sandbox under repository-defined policy, validate dependencies and runtime behavior, and return structured evidence to the maintainer.

The POC should demonstrate:
- PR-controlled build/test code cannot access host credentials;
- unauthorized network egress is blocked and surfaced as evidence;
- unapproved dependency sources can fail an explicit policy;
- architecture or scope findings can request human review;
- only deterministic, explicit repository policy can hard-fail the TrustForge check.

See [prototypes/openshell-pr-evaluator/README.md](prototypes/openshell-pr-evaluator/README.md).

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
│   ├── isolation-model.md
│   ├── policy-model.md
│   ├── threat-model.md
│   └── use-cases/
│
├── research/
│   ├── ecosystem-evidence.md
│   └── references.md
│
├── specs/
│   ├── contribution-context.md
│   ├── review-evidence.md
│   ├── risk-signals.md
│   └── policy-decision.md
│
├── prototypes/
│   └── openshell-pr-evaluator/
├── examples/
└── .github/
    ├── ISSUE_TEMPLATE/
    └── workflows/
```

## Project status

**Exploration / pre-alpha.**

The first milestone is not to build a large AI review system. It is to validate the problem, define an interoperable review-evidence model, prove that untrusted PR evaluation can be safely isolated, and test whether that model measurably reduces maintainer effort.

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
- isolation runtime integrations;
- repository policy models;
- integrations with existing CI/security tools;
- evaluation methodology.

## License

Apache License 2.0.
