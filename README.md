# atltuae

> The world does not lack answers; it lacks *honest maps of how much we actually know and how much we do not.* Synthesis engines and "research agents" today optimize for the confident-sounding final out  ·  **Risk tier:** med  ·  **Status:** planning

The world does not lack answers; it lacks *honest maps of how much we actually know and how much we do not.* Synthesis engines and "research agents" today optimize for the confident-sounding final output: they conclude fast, cite loosely, and hide their coverage gaps. ATLTUAE inverts that. It is an engine whose **deliverable is the cited synthesis itself plus a public ledger of its own ignorance**, and whose hardest-wired rule is *do not emit a final answer until the corpus is provably exhausted.* Because "all domains" is never exhausted, the engine's natural state is **perpetual, disciplined improvement** — broader breadth, deeper depth, stronger verification — not conclusion.

**Definition of shipped:** synthesis with a public coverage ledger** that an **independent party can re-derive** and that a **real audience or partner uses or builds on** — produced by a loop that demonstrably never concluded prematurely, never exceeded budget, and never ingested a closed source. (Adoption

This is an **Elyos** good-deed project. Contributors pull a task, do it with their own coding agent, and open a PR. Platform: https://github.com/jdev1977/elyos

## Plan
- [PLAN.md](./PLAN.md) — robust enterprise plan (vision, architecture, roadmap, risks; includes an applied-improvements appendix + review sign-off)
- [TASKS.md](./TASKS.md) — schema-mapped task backlog
- [tasks/](./tasks/) — ready-to-pull task JSON(s)

## Contribute
```bash
elyos browse
elyos next --repo Elyos-Projects/atltuae --no-fork
```

## Licensing & review
- Open license (see PLAN.md).
- Risk tier **med** — deeds are *delivered, not merged*; a domain reviewer (and expert sign-off for any high-stakes content) must approve before merge.

> Planning stage; no adopting partner secured yet (`verifiedNeed: false` on delivery-dependent tasks).
