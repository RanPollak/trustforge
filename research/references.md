# References

Curated references for TrustForge research. Prefer primary sources.

## Problem / ecosystem pressure

- GitHub Octoverse 2025 — https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/
- GitHub — How pull request limits are cutting down the noise (2026) — https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/
- GitHub — Inside the Advisory Database (2026) — https://github.blog/security/supply-chain-security/inside-the-advisory-database-and-what-happens-when-vulnerability-volume-breaks-records/

## Adjacent PR/review projects

- PR-Agent — https://github.com/qodo-ai/pr-agent
- Danger — https://github.com/danger/danger
- OSS Review Toolkit (ORT) — https://github.com/oss-review-toolkit/ort

## Isolation / runtime

- NVIDIA OpenShell — https://github.com/NVIDIA/OpenShell
- OpenShell sandbox architecture — https://github.com/NVIDIA/OpenShell/blob/main/architecture/sandbox.md
- OpenShell policy docs — https://docs.nvidia.com/openshell/sandboxes/policies
- OpenShell policy quickstart — https://github.com/NVIDIA/OpenShell/tree/main/examples/sandbox-policy-quickstart
- OpenShell issue #2750 — privileged supervisor/workload-image trust boundary — https://github.com/NVIDIA/OpenShell/issues/2750
- StepSecurity Harden-Runner — https://github.com/step-security/harden-runner

OpenShell is the **reference isolation provider for the first TrustForge POC**, not a required dependency of the portable evidence model.

## Static analysis / dependency evidence

- Semgrep — https://github.com/semgrep/semgrep
- OSV-Scanner — https://github.com/google/osv-scanner
- GitHub Dependency Review Action — https://github.com/actions/dependency-review-action
- OpenSSF Scorecard — https://github.com/ossf/scorecard

## Policy

- Open Policy Agent — https://github.com/open-policy-agent/opa
- Conftest — https://github.com/open-policy-agent/conftest

## Provenance / supply-chain evidence

- Sigstore — https://github.com/sigstore
- Sigstore Policy Controller — https://github.com/sigstore/policy-controller
- in-toto — https://github.com/in-toto/in-toto
- GUAC — https://github.com/guacsec/guac

## TrustForge positioning rule

These projects are references and potential providers, not a feature checklist to reproduce.

TrustForge should own:
- contribution-bound evidence;
- provider/isolation contracts;
- normalization;
- provenance of review evidence;
- policy-decision semantics;
- orchestration;
- maintainer presentation;
- evaluation of reviewer impact.

See [../docs/ecosystem-map.md](../docs/ecosystem-map.md).
