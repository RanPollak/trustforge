# TrustForge Providers

TrustForge providers adapt existing tools into normalized review evidence.

> **A provider is preferred over reimplementing the underlying capability in TrustForge core.**

## Status

No provider listed here should be considered production-supported yet.

The project is pre-alpha and the first goal is to validate provider contracts through a small vertical-slice POC.

## Candidate providers

| Provider/tool | Category | Intended role | Status |
| --- | --- | --- | --- |
| OpenShell | isolation/runtime | Reference isolation provider | POC target |
| Existing GitHub/CI checks | ci | Reuse project validation | POC target |
| OSV-Scanner | dependency | Vulnerability evidence | POC candidate |
| Semgrep | static_analysis | Static/security findings | POC candidate |
| OPA/Conftest | policy | Deterministic project policy | POC candidate |
| Sigstore / in-toto | provenance | Signature/attestation evidence | Later/optional POC |
| GitHub Dependency Review | dependency | PR dependency-diff evidence | Candidate |
| OpenSSF Scorecard | dependency/posture | Project security posture | Candidate |
| GUAC | supply_chain_context | Supply-chain relationship context | Candidate |
| ORT | dependency/compliance | Dependency/compliance evidence | Candidate |
| PR-Agent / similar | reasoning | Optional PR reasoning/context | Candidate, non-authoritative |
| Harden-Runner / similar | runtime | CI runtime observations | Candidate |

## Adapter expectations

Every provider adapter should document:

- provider ID;
- provider category;
- provider/adapter version;
- execution class;
- inputs;
- normalized evidence kinds;
- raw artifact/reference;
- failure semantics;
- isolation requirements;
- data egress/privacy requirements.

See:

- [../specs/evidence-provider.md](../specs/evidence-provider.md)
- [../specs/isolation-provider.md](../specs/isolation-provider.md)
- [../docs/integration-strategy.md](../docs/integration-strategy.md)

## Directory convention (future)

```text
providers/
├── isolation/
│   └── openshell/
├── static-analysis/
│   └── semgrep/
├── dependency/
│   └── osv-scanner/
├── policy/
│   └── conftest/
├── provenance/
└── reasoning/
```

Do not create empty provider implementations merely to fill this structure. Add an adapter when its contract and POC are ready.
