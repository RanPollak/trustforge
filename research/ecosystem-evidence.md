# Ecosystem evidence baseline

This document captures the public evidence behind TrustForge's problem statement. Update as primary sources evolve.

## Headline observations

| Claim | Source | Notes |
| --- | --- | --- |
| 518.7 million merged PRs in 2025 (+29% YoY) | GitHub Octoverse 2025 | Contribution volume scaling |
| Issue/PR comments essentially flat (+0.35%) | GitHub Octoverse 2025 | Review/discussion capacity not matching volume |
| PR limits introduced to cut noise / review overhead | GitHub blog, June 18, 2026 | Maintainer load and low-quality noise |
| Vulnerability advisory volume at record levels | GitHub blog, June 29, 2026 | Supply-chain / security review pressure |

## Primary sources

- GitHub Octoverse 2025: https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/
- GitHub, "How pull request limits are cutting down the noise" (June 18, 2026): https://github.blog/open-source/maintainers/how-pull-request-limits-are-cutting-down-the-noise/
- GitHub, "Inside the Advisory Database and what happens when vulnerability volume breaks records" (June 29, 2026): https://github.blog/security/supply-chain-security/inside-the-advisory-database-and-what-happens-when-vulnerability-volume-breaks-records/

## Interpretation for TrustForge

These sources support the thesis that **production of changes is outpacing review capacity**, and that tooling responses are already appearing (e.g. PR limits). TrustForge's response is **evidence infrastructure for maintainer judgment**, not further automation of merge decisions.

## Gaps / next research

- [ ] Maintainer interview notes (anonymized themes)
- [ ] Quantitative review-latency datasets suitable for evaluation
- [ ] Mapping of existing review bots and where they add noise vs. signal
- [ ] Related academic and industry work — see [references.md](references.md)
