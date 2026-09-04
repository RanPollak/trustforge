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
   CODEOWNERS, required checks, branch protection, and project norms override generic defaults.

6. **Security is a first-class review dimension.**  
   Auth, crypto, privilege, dependency, and supply-chain changes are surfaced explicitly.

7. **No universal "trust score."**  
   Scores hide trade-offs and invite gaming. Prefer structured evidence packages.

8. **Reduce noise before adding more review output.**  
   Deduplicate, collapse, and prioritize. More text is not more trust.

9. **Open standards and interoperable integrations.**  
   Evidence formats should be portable across tools and forges where practical.

10. **Measure whether we actually save maintainer time.**  
    If the evidence package does not reduce effort or improve decision quality, it is not done.

## Anti-patterns

- Opaque contributor reputation scores based on personal attributes
- Treating model confidence as proof of safety
- Bypassing required human review
- Flooding PRs with unverified bot commentary
- Replacing CODEOWNERS or governance with a generic policy engine
