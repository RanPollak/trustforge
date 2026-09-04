# Core principles

1. **Maintainers stay in control.**
   TrustForge informs decisions; it does not make them. Automatic approve/merge is out of scope for v0.

2. **Evidence before judgment.**
   Present auditable inputs before probabilistic commentary.

3. **Deterministic checks before probabilistic reasoning.**
   CI, tests, scanners, policy engines, and provenance come first. LLMs may help synthesize or explain; they are not security evidence.

4. **Explain every signal.**
   Each finding should say what was observed, why it matters, where it came from, and how to inspect the source evidence.

5. **Project policy is authoritative.**
   CODEOWNERS, required checks, branch protection, repository policy, and community governance override generic defaults.

6. **Security is a first-class review dimension.**
   Auth, crypto, privilege, dependency, runtime behavior, provenance, and supply-chain changes are surfaced explicitly.

7. **No universal "trust score."**
   A single score hides trade-offs and invites gaming. Prefer structured evidence.

8. **Reduce noise before adding more review output.**
   Deduplicate, collapse, prioritize, and route. More generated text is not more trust.

9. **Open standards and interoperable integrations.**
   Evidence should be portable across tools, isolation runtimes, and forges.

10. **Measure whether we actually save maintainer time.**
    If TrustForge does not reduce effort, improve decision quality, or reduce review noise, it is not done.

11. **Never execute untrusted contribution code without isolation.**
    PR-controlled build scripts, tests, package hooks, generated code, and dependencies are potentially hostile.

12. **Integrate before inventing.**
    If a mature open-source tool already produces a useful signal or execution capability, TrustForge should prefer an adapter over a replacement implementation.

13. **Normalize evidence, not tool identity.**
    TrustForge should preserve provider identity and provenance while exposing portable evidence semantics. A finding is not trusted merely because it came from a well-known tool.

## Policy principle

TrustForge distinguishes **hard enforcement** from **advisory judgment**:

- explicit deterministic repository rules may produce `POLICY_FAILED`;
- contextual or probabilistic findings produce `NEEDS_REVIEW` unless the project has codified the rule;
- model-only findings never become hard merge blockers.

## Duplication guardrail

Before adding a new core subsystem, answer:

1. Which existing open-source projects already solve this part?
2. Can their result be normalized through an `EvidenceProvider`?
3. Can execution be delegated through an `IsolationProvider`?
4. What genuinely new semantic or orchestration capability would remain in TrustForge?

If the answer is "TrustForge would mostly reproduce an existing engine," the feature probably does not belong in core.

## Anti-patterns

- Reimplementing SAST, dependency scanning, policy languages, signing, or sandboxing without a demonstrated gap
- Opaque contributor reputation scores based on personal attributes
- Treating model confidence as proof of safety
- Bypassing required human review
- Flooding PRs with unverified bot commentary
- Replacing CODEOWNERS or governance with generic automation
- Running untrusted PR code directly on the host or with ambient credentials
- Allowing a PR to weaken the policy used to evaluate that same PR
- Automatically closing/rejecting contributions based only on probabilistic judgment
