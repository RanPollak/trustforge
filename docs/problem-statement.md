# Problem statement

Open source development is entering a phase where the ability to **produce** software changes is scaling faster than the community's ability to **understand, validate, review, and safely accept** them.

GitHub reported **518.7 million merged pull requests in 2025, up 29% year over year**, while comments on issues and pull requests were essentially flat at **+0.35%**. In 2026, GitHub introduced pull-request limits specifically to help maintainers manage increasing contribution volume, low-quality noise, and review overhead.

## What is breaking

Maintainer capacity is not growing at the same rate as contribution volume. The result is not only slower merges — it is:

- more unreviewed or lightly reviewed risk;
- more noise that displaces high-value review;
- more pressure to automate judgment rather than improve evidence;
- more difficulty routing specialist review (security, auth, build systems) to the right people.

## What the problem is not

The problem is not merely "generate code faster" or "summarize diffs with an LLM."

Summaries without durable, auditable evidence do not establish trust. Opaque scores hide the basis of judgment. Automatic approve/merge shifts liability without improving understanding.

## What the problem is

The core challenge is **establishing trust at scale**: turning incoming contributions into structured **review evidence** so maintainers can make well-informed decisions faster — without surrendering control.

See also:

- [vision.md](vision.md)
- [principles.md](principles.md)
- [../research/ecosystem-evidence.md](../research/ecosystem-evidence.md)
