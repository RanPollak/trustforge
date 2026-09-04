# Core principles

1. **Maintainers stay in control.**
   TrustForge informs decisions; it does not make them. Automatic approve/merge is out of scope for v0.

2. **Evidence before judgment.**
   Present auditable inputs (context, validation, risk) before any probabilistic commentary.

3. **Deterministic checks before probabilistic reasoning.**
   CI, tests, linters, static analysis, policy, and provenance come first. LLMs may help narrate or organize; they are not security evidence.

4. **Explain every signal.**
   Each risk or triage signal should say what was observed, why it matters, and how to verify it.

5. **Project policy is authoritative.**
   CODEOWNERS, required checks, branch protection, repository policy, and project norms override generic defaults.

6. **Security is a first-class review dimension.**
   Auth, crypto, privilege, dependency, runtime behavior, and supply-chain changes are surfaced explicitly.

7. **No universal "trust score."**
   Scores hide trade-offs and invite gaming. Prefer structured evidence packages.

8. **Reduce noise before adding more review output.**
   Deduplicate, collapse, and prioritize. More text is not more trust.

9. **Open standards and interoperable integrations.**
   Evidence formats should be portable across tools, isolation runtimes, and forges where practical.

10. **Measure whether we actually save maintainer time.**
    If the evidence package does not reduce effort or improve decision quality, it is not done.

11. **Never execute untrusted contribution code without isolation.**
    PR-controlled build scripts, tests, package hooks, generated code, and dependencies must be treated as potentially hostile. Execution belongs in a restricted disposable environment without ambient host credentials and with controlled network/filesystem access.

## Policy principle

TrustForge distinguishes **hard enforcement** from **advisory judgment**:

- explicit deterministic repository rules may produce `POLICY_FAILED`;
- contextual or probabilistic findings produce `NEEDS_REVIEW` unless the project has codified the rule;
- model-only findings never become hard merge blockers.

## Anti-patterns

- Opaque contributor reputation scores based on personal attributes
- Treating model confidence as proof of safety
- Bypassing required human review
- Flooding PRs with unverified bot commentary
- Replacing CODEOWNERS or governance with a generic policy engine
- Running untrusted PR code directly on the host or with ambient credentials
- Allowing a PR to weaken the policy used to evaluate that same PR
- Automatically closing/rejecting contributions based only on probabilistic judgment
