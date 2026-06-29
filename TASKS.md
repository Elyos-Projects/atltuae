# ATLTUAE — TASKS.md

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-29 · Owner: TBD (maintainer) · Lane: donated (funded cycles allowed, with a hard per-cycle cap)

Backlog for **ATLTUAE** (slug: `atltuae`), a self-improving, perpetually-looping **open research +
analysis engine** that broadens coverage across all domains and maintains a **perpetual living
synthesis** — releasing/promoting nodes via a **frontier-saturation gate** *and* verification gate
rather than ever declaring the corpus globally exhausted. The deliverable is the **ever-improving,
fully-cited open synthesis** plus a **public coverage ledger**. See `PLAN.md` for full context.

The project is built on four refusals that are hard product requirements and CI release gates:
**no premature conclusion, no headless/unattended run, no unsourced claim, no closed/unlicensed
source.** The **M0 spine is the coverage-gate + provenance + budget-cap loop.** This is a **MEDIUM**
risk-tier project overall; any surface the domain-risk router tags health/legal/safety/financial is
**HIGH** and requires credentialed-expert sign-off + "not advice" framing before it appears in a
published synthesis. No partner/steward/expert is yet secured, so delivery-dependent tasks carry
`requestor: TO BE SECURED` and `verifiedNeed: false`.

## How these tasks map to Elyos

Each task below becomes an Elyos **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable slug `atltuae-<area>-NNN` (e.g. `atltuae-spec-000`).
- **title** — the task title in the milestone table.
- **project** — `atltuae`.
- **type** — one of `code | research | writing | data | design-spec | maintenance`.
- **lane** — `donated` (default). **Funded** cycles must add `fundedBudgetUsd` (a hard per-cycle cap);
  one funded task is included (`atltuae-cost-017`) to demonstrate the contract.
- **priority** — `high | medium | low`.
- **domain** — array, e.g. `["research","knowledge-commons","software","provenance"]`.
- **riskTier** — `low | medium | high`. The gate/budget/provenance/verification subsystems are
  safety-critical (treated `high` where a bypass would break a refusal); domain-risk routing is
  `high`; general engine/infra is `low–medium`.
- **urgent** — boolean (no urgent tasks at cold-start).
- **deliverable** — `pr | dataset | document | translation`.
- **tokenEstimate** — `small | medium | large` (maps to the Size column).
- **status** — `open | in-progress | review | delivered | done` (all start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — partner/steward/expert; `TO BE SECURED` where unknown.
- **verifiedNeed** — `true` only once a specific partner/need is confirmed; currently `false`
  everywhere (no partner secured).
- **outputLicense** — code: **MIT**; synthesis/ledger/datasets/docs: **CC-BY-4.0**.

Size legend: small ≈ tokenEstimate `small`, med ≈ `medium`, large ≈ `large`.
Reviewer "Gate reviewer" = the independent gate/red-team/budget reviewer; "Expert" = credentialed
domain reviewer for HIGH-tier surfaces (**TO BE SECURED**).

---

## Milestone M0 — The spine: coverage ledger + provenance + budget-cap loop (cold-start)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-spec-000 | Loop-charter / responsible-loop policy spec (4 refusals; caps; checkpoints; provenance) | design-spec | medium | high | document | — | Maintainer + Gate reviewer |
| atltuae-seed-001 | Seed-domain selection (scored vs. criteria; low-risk, source-rich) — gates later cycles | research | small | medium | document | — | Maintainer |
| atltuae-repo-002 | Monorepo + pnpm + TS/ESM + CI (build/test/lint) skeleton; agent-neutral core + adapters seam | code | small | low | pr | — | Maintainer |
| atltuae-provenance-003 | Provenance store + "no source, no claim" + sampled faithfulness/entailment gate (content-addressed archived snapshot; license + verify status; Citations spans) | code | large | high | pr | 002 | Maintainer + Gate reviewer |
| atltuae-budget-004 | Budget governor: hard per-cycle caps (tokens/USD/sources/wall-clock) + aggregate/lifetime ceiling + marginal-coverage-per-dollar tripwire, fail-closed stop | code | medium | high | pr | 002 | Gate reviewer |
| atltuae-ledger-005 | Coverage ledger schema + public renderer (taxonomy; covered/partial/missing with quality floor; non-empty frontier) | code | medium | medium | pr | 002 | Maintainer |
| atltuae-loop-006 | Minimal cycle orchestrator (one bounded cycle on seed domain) + mandatory human checkpoint; never headless | code | large | high | pr | 003, 004, 005 | Maintainer + Gate reviewer |

**Acceptance criteria — key tasks**

- **atltuae-spec-000** (loop-charter / responsible-loop policy spec)
  - States the **four refusals** in concrete, testable terms: (1) **no premature conclusion** — no
    final answer while any frontier node or unverified/contradicted claim remains; (2) **no headless
    run** — every expansion round gated by a human checkpoint, no continuous/unattended process; (3)
    **no unsourced claim** — every surfaced claim carries verifiable, license-clean provenance; (4)
    **no closed/unlicensed source** — open/PD/CC only, verified before ingestion.
  - Defines the per-cycle **budget-cap dimensions** (tokens, USD, source count, wall-clock), the
    **aggregate/lifetime ceiling + marginal-coverage-per-dollar tripwire**, and the **fail-closed** stop
    behavior (partial work preserved; no in-cycle override; cap raises are a governance decision).
  - Defines **faithfulness (entailment) as a first-class gate distinct from citation coverage** — the
    cited source must actually support the claim (presence ≠ faithfulness).
  - Defines the **dual frontier-saturation+verification gate** as the sole node-level release/promotion
    arbiter, defaulting fail-closed to "not yet," and the operational definition of "saturated" (to be
    parameterized in M1, including the **search-recall / source-diversity floor** that must be cleared
    before "no new sources" can count); states there is **no global "final answer" door**.
  - Specifies the **tiered human-checkpoint model** (batched lightweight approval for low-risk breadth;
    full review for high-risk routing / self-improvement diffs / budget changes).
  - Specifies that **self-improvement is quality-only** (taxonomy/method/prompt) and **may never**
    modify budget caps, gates, or refusals.
  - Specifies **domain-risk routing** ("not advice" + expert flag for health/legal/safety/financial)
    and **non-partisan** handling for civic topics.
  - Documents the **"42" easter egg** as code/README-only, never surfaced in the public synthesis.
  - Reviewed and signed off by the gate reviewer (recorded).

- **atltuae-provenance-003** (provenance store + "no source, no claim" + faithfulness)
  - Every claim links to ≥ 1 `Source` (identifier/URL, title, **license + verified reuse status**,
    retrieval date, **content/snapshot hash**, **exact source span via Anthropic Citations**,
    derivation/query trail) and a verification status.
  - The synthesis layer **cannot** surface a claim lacking provenance; a **citation-coverage test**
    fails the build if any surfaced claim is unsourced.
  - A **separate sampled faithfulness/entailment gate** checks that the cited snapshot *actually
    supports* the claim (presence ≠ faithfulness — the Galactica/ProVe failure); unfaithful claims are
    withheld/flagged. Faithfulness is reported distinctly from coverage.
  - Sources and claims are **content-addressed against an archived snapshot** so a claim is
    **re-derivable even after link rot** (reproducibility foundation); provenance chain is tamper-evident.

- **atltuae-budget-004** (budget governor)
  - Enforces per-cycle caps on tokens, USD, source count, and wall-clock; meters every LLM/fetch call
    against remaining budget and **refuses** the call that would exceed it.
  - Enforces a **governance-owned aggregate/lifetime budget ceiling** and a **marginal-coverage-per-
    dollar tripwire** that fires a continue/narrow/mothball decision when new spend stops buying
    meaningful quality-weighted coverage (cost-driven complement to the 2027 partner rule).
  - On cap trip, the cycle **stops fail-closed** with partial work persisted and a truncation note
    written to the ledger; a **cap-trip test** proves the stop occurs and nothing runs past the cap.
  - **No in-cycle override path** for the per-cycle cap or the lifetime ceiling; raising either is a
    between-cycle, governance-logged human action.

- **atltuae-loop-006** (minimal cycle orchestrator + human checkpoint)
  - Runs one bounded cycle (plan → gather → verify → synthesize → ledger → checkpoint) on the seed
    domain; **halts at the checkpoint** and **requires explicit human approval** to start the next
    cycle — there is **no auto-advance and no headless mode**.
  - Emits **no final answer** (frontier non-empty); publishes a coverage ledger; respects the budget
    governor (cap-trip honored) and the provenance rule (no unsourced claim surfaced).
  - An **audit log** records that every expansion round was human-gated (checkpoint adherence = 100%).

**M0 Definition of Done:** the loop-charter spec (gate-reviewed) is merged; the spine runs **one
bounded, human-gated cycle on the seed domain** in which every claim has verifiable license-clean
provenance (citation-coverage test green), the budget cap is enforced fail-closed (cap-trip test
green), a public coverage ledger with a non-empty frontier is published, the human checkpoint gates
the next cycle (audit log proves it), and **no final answer is emitted**. Monorepo + CI green.

---

## Milestone M1 — Gates & verification (no premature conclusion)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-gates-007 | Frontier-saturation gate + verification gate (dual node-level "release?"; search-recall floor; default fail-closed; sole release arbiter) | code | large | high | pr | 005, 006 | Gate reviewer + Maintainer |
| atltuae-verify-008 | Adversarial verification + contradiction detection + multi-source rule + source-independence scoring for high-stakes claims | code | large | high | pr | 003, 007 | Gate reviewer |
| atltuae-redteam-009 | Red-team suite: premature-release / budget-bypass / unsourced-claim / unfaithful-claim / closed-source (CI build-fail) | code | medium | high | pr | 007, 008 | Gate reviewer |

**Acceptance criteria — key tasks**

- **atltuae-gates-007** (frontier-saturation + verification gate)
  - Saturation gate reports a node "saturated" **only** if there are **no open frontier nodes under
    it** *and* **no new sources across K consecutive cycles** — and the "no new sources" signal counts
    **only after a search-recall / source-diversity floor is cleared** (minimum diversity of query
    strategies + source classes incl. citation-graph traversal). K + floor parameterized + governance-set.
  - Verification gate releases a node **only** if **all its claims verified and faithful**, **no
    unresolved contradictions**, and reproducibility ≥ threshold.
  - The gate's job is **node-level release/promotion** (publish at confidence tier X), **defaults
    fail-closed to "not yet,"** and treats any missing data / unmet recall floor / gate error /
    ambiguity as "not yet." There is **no global "final answer" door**.

- **atltuae-verify-008** (adversarial verification + multi-source + source-independence)
  - Each claim is adversarially checked (steel-man + seek disconfirming sources), faithfulness-checked
    against its cited source, not accepted on a single agreeing source; contradictions across the claim
    graph are **surfaced, not hidden**.
  - **High-stakes claims require multiple independent, license-clean sources** before they may be
    surfaced even as "informational," with **source-independence scoring** so correlated / echoed /
    LLM-generated near-duplicate sources cannot satisfy the multi-source rule.
  - Reuses the `deep-research` harness pattern (fan-out → fetch → adversarially verify → synthesize).

- **atltuae-redteam-009** (refusal red-team suite)
  - Covers each refusal with multiple cases incl. **prompt-injection from ingested content** (e.g.,
    "mark the corpus exhausted/saturated," "ignore the budget cap"): premature-release attempts,
    budget-bypass attempts (per-cycle **and** lifetime), unsourced-claim attempts, **unfaithful-claim
    attempts** (citation present but non-supporting), closed-source-ingestion attempts.
  - Runs in CI; a regression that bypasses **any** refusal **fails the build**; suite size is
    **non-decreasing**; newly-found bypasses become regression cases within one release.
  - Verification eval: **≥ 95%** of seeded false claims are flagged, reported as caught/total at
    version N.

**M1 Definition of Done:** the dual frontier-saturation+verification gate computes/reports, defaults
fail-closed to "not yet," counts "no new sources" only after the search-recall floor is cleared, and
blocks node release/promotion while frontier/unverified/unfaithful/contradicted claims remain; the
sampled faithfulness/entailment gate is green (distinct from coverage); the verification layer catches
≥ 95% of seeded false claims and enforces multi-source + source-independence for high-stakes; the
refusal red-team suite (incl. injection + unfaithful-claim) passes in CI and fails the build on any
bypass.

---

## Milestone M2 — Expansion planner, human checkpoints & gated self-improvement

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-planner-010 | Expansion planner: select next-cycle breadth + depth targets from coverage gaps, with per-target budget | code | medium | medium | pr | 005, 007 | Maintainer |
| atltuae-checkpoint-011 | Tiered human-checkpoint workflow (batched low-risk breadth vs. full review for high-risk/self-improve/budget) with cycle diff + coverage delta + cost | code | medium | high | pr | 006, 010 | Maintainer + Gate reviewer |
| atltuae-improve-012 | Gated self-improvement: engine proposes taxonomy/method/prompt diffs; human-approved; cannot touch budget/gates/refusals | code | large | high | pr | 011 | Gate reviewer + Maintainer |

**Acceptance criteria — key tasks**

- **atltuae-planner-010** (expansion planner)
  - Reads the coverage ledger and proposes next-cycle **breadth** (new nodes) + **depth**
    (under-covered nodes) targets, each with an estimated budget that fits within the per-cycle cap.
  - Never proposes concluding; the frontier remains explicit and non-empty.

- **atltuae-checkpoint-011** (tiered human-checkpoint workflow)
  - At each checkpoint a human can **approve / modify / halt** the next round; presents a **cycle
    diff, coverage delta, and cost summary** for an informed decision.
  - **Tiered:** low-risk breadth expansion can be **batch-approved** lightweight; **full human review
    is required** for high-risk domain routing, self-improvement diffs, and budget/cap changes — so the
    gate scales to "all domains" without becoming a bottleneck or fatigued rubber stamp.
  - No round advances without an explicit human action (no auto-advance); the decision + tier is logged.

- **atltuae-improve-012** (gated self-improvement)
  - The engine may propose improvements to **taxonomy, retrieval/verification methods, and prompts**,
    each as a reviewable diff + rationale, applied **only after human approval** at a checkpoint.
  - The engine **provably cannot** propose or apply changes to **budget caps, gates, or refusal
    rules** (enforced server-side; covered by a red-team case) — self-improvement is quality-only.

**M2 Definition of Done:** a multi-cycle run measurably broadens coverage (ledger diff shows breadth +
depth growth and an always-non-empty frontier); the approve/modify/halt checkpoint workflow works and
gates every round (audit log); self-improvement proposals are human-approved and provably cannot touch
budget/gates/refusals (red-team case green).

---

## Milestone M3 — Domain-risk routing + licensing/privacy hardening

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-risk-013 | Domain-risk router: tag health/legal/safety/financial → "not advice" + expert-review flag; non-partisan civic handling | code | medium | high | pr | 007, 008 | Expert + Gate reviewer |
| atltuae-license-014 | License gate: verify reuse terms before ingestion (open/PD/CC only); reject closed/ToS/controlled; share-alike lineage | code | medium | high | pr | 003 | Maintainer + Gate reviewer |
| atltuae-privacy-015 | PII / privacy filter: block personal-data ingestion + republication; aggregate/PD/CC only | code | medium | medium | pr | 003, 014 | Maintainer |

**Acceptance criteria — key tasks**

- **atltuae-risk-013** (domain-risk router)
  - Classifies each claim/surface into a risk domain; health/legal/safety/financial are tagged HIGH,
    labeled **"informational, not advice,"** and **flagged for credentialed-expert review** before
    appearing in a published synthesis (no expert sign-off → that surface is withheld).
  - Civic/contested topics handled **non-partisan** (sourced positions mapped, not advocated);
    verified on a civic test set.

- **atltuae-license-014** (license gate)
  - **Before** ingestion, verifies and records each source's reuse terms; **rejects** unknown/closed/
    paywalled/controlled-access/ToS-restricted sources (test asserts **0** such sources ingested).
  - Records per-source license lineage incl. **share-alike** (CC-BY-SA/ODbL) so the synthesis license
    strategy can isolate/attribute share-alike sections; robots/ToS respected for fetching.

**M3 Definition of Done:** the domain-risk router tags 100% of health/legal/safety/financial claims
with "not advice" + expert flag; the license gate rejects 100% of closed/unlicensed/ToS sources in
tests (0 ingested) and records share-alike lineage; the PII filter blocks personal-data ingestion/
republication; non-partisan handling verified on a civic test set.

---

## Milestone M4 — Reproducibility + public synthesis + cost ledger

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-repro-016 | Reproducibility tooling: independent re-run re-derives sampled claims from archived snapshots; link-rot/decay metric + stale-claim re-verification queue | code | medium | high | pr | 003, 008 | Provenance auditor + Maintainer |
| atltuae-cost-017 | Public cost-ledger renderer for **funded** cycles (spend vs. cap; 0 over-cap) — funded-lane demo | code | small | medium | pr | 004, 005 | Gate reviewer + Maintainer |
| atltuae-synthesis-018 | Publish the ever-improving cited synthesis + coverage dashboard (CC-BY) with re-derivation guide | writing | large | high | document | 016, 013 | Maintainer + Expert (HIGH surfaces) |

**Acceptance criteria — key tasks**

- **atltuae-repro-016** (reproducibility)
  - An **independent re-run** (no access to prior intermediate state) re-derives **≥ 95%** of a random
    sample of published claims from the recorded **archived snapshots**/queries (judged against the
    snapshot, not the live web, to survive link rot); misses are investigated/logged.
  - A **link-rot / source-decay metric** tracks references that have since moved/disappeared and a
    **stale-claim re-verification queue** schedules re-checking aged claims against fresh snapshots.
  - The re-derivation procedure is documented so external parties can reproduce it.

- **atltuae-cost-017** (public cost ledger — funded demo)
  - For funded cycles, renders a **public cost ledger**: per-cycle USD spend vs. the hard cap **and
    aggregate/lifetime spend vs. the governance ceiling**, reconciled against the budget governor;
    **0 over-cap** cycles; no secrets/credentials in the ledger. (This task is `lane: funded` and
    carries `fundedBudgetUsd` — see schema rule.)

- **atltuae-synthesis-018** (public synthesis + dashboard)
  - Publishes the cited synthesis + coverage ledger as **CC-BY-4.0** artifacts with full provenance
    and the re-derivation guide; **no final answer** is asserted (frontier published non-empty).
  - Any HIGH-tier (health/legal/safety/financial) surface present carries "not advice" + recorded
    expert sign-off, or is withheld.

**M4 Definition of Done:** an independent re-run re-derives ≥ 95% of a sampled claim set; the public
cost ledger reconciles spend vs. cap with 0 over-cap cycles; the cited synthesis + coverage dashboard
are published CC-BY with a re-derivation guide and a non-empty frontier; HIGH surfaces expert-signed
or withheld.

---

## Milestone M5 — Sustain, community & governance (post-delivery / the deed)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
|---|---|---|---|---|---|---|---|
| atltuae-partner-019 | Secure partner/steward + publish synthesis to a real audience; outcome recorded (the deed) | research | medium | high | document | 018 | Steward + Maintainer |
| atltuae-ops-020 | Ops runbook + maintenance rotation + cycle cadence + governed process for community domains & cap/gate changes | maintenance | medium | medium | document | 019 | Maintainer + Steward |

**Acceptance criteria — key tasks**

- **atltuae-partner-019** (secure partner + the deed)
  - A real partner/audience (open-science org, library/archive, university group, or open-knowledge
    community) adopts or publishes the synthesis/ledger; steward assigned; `verifiedNeed` flips to
    `true`; the use/hand-off is recorded.
  - Driven by the **dated partner-acquisition plan** (≥ 3 candidates in conversation by 2026-09-30,
    steward by 2026-12-31). If none is secured by **~2027-03-31**, apply the **continue-vs-mothball/
    pivot decision rule** (PLAN exec summary): pivot the deliverable to an open-knowledge community as
    a reusable tool/reference corpus, or **mothball** (pause the loop, freeze + publish the ledger as a
    final-state artifact) — recorded in governance, never loop forever for no audience.

- **atltuae-ops-020** (sustain & governance)
  - Runbook covers deploy, running a cycle, incident response, source re-verification, and the
    fail-closed budget behavior; named maintenance rotation; documented **cycle cadence** (not
    continuous).
  - Documents the **governed process** for community-proposed domains and for any **budget-cap/gate/
    refusal change** (human + governance only; never engine self-modified).

**M5 Definition of Done:** project-level **Definition of Shipped** met — a published, openly-licensed,
fully-cited synthesis + public coverage ledger, independently re-derivable, used by or handed to a
real audience/partner, produced by a loop that never concluded prematurely, never exceeded budget,
never ingested a closed source, and gated every round on a human; sustainably maintained with a
documented cadence and governed change process. *(Gated on a secured partner — TO BE SECURED.)*

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
|---|---|---|---|---|---|---|
| atltuae-federation-021 | Multi-engine federation (parallel domain engines sharing one ledger/provenance store) | code | large | medium | pr | Only after M5; scales breadth without relaxing caps/gates |
| atltuae-i18n-022 | Translate the public synthesis into additional languages (community-reviewed) | writing | medium | medium | translation | Per-language review; provenance preserved. **Fan-out note:** target languages are not yet enumerated (depend on the secured audience/community), so this is one **representative** task (`type: writing`, `deliverable: translation`); it expands to one task per **confirmed** language once an audience + fluent reviewers exist — no languages fabricated in advance. License is source-compatible (CC-BY-4.0 source → CC-BY-4.0 translation; never relicense a copyrighted source). |
| atltuae-seed-deeds-023 | Use coverage gaps to auto-propose new Elyos good-deed projects | research | medium | low | document | ATLTUAE as a meta-tool seeding other deeds |
| atltuae-dataset-024 | Publish the claim/provenance graph as a reusable open dataset | data | medium | medium | dataset | CC-BY-4.0; datasheet + license lineage |
| atltuae-eval-025 | Continuous verification/reproducibility eval dashboard across cycles | code | medium | medium | pr | Tracks provenance/refusal integrity over time |

---

## Example task JSON

Complete, schema-valid Task JSON for the first M0 task (`atltuae-spec-000`):

```json
{
  "id": "atltuae-spec-000",
  "title": "Loop-charter / responsible-loop policy specification (the four refusals; budget caps; human checkpoints; provenance)",
  "project": "atltuae",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["research", "knowledge-commons", "provenance", "ai-safety", "software"],
  "riskTier": "high",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "ATLTUAE is a self-improving, perpetually-looping open research+analysis engine whose deliverable is an ever-improving, fully-cited synthesis plus a public coverage ledger - NOT a final answer. Its hardest rule is that it must not emit a final answer until its coverage gate AND verification gate report the corpus exhausted (across all domains, effectively never). The danger of a 'perpetual loop' is that it becomes an unbounded, headless autonomous agent; Elyos rules forbid running an agent unattended or exceeding a hard budget cap. The M0 spine reconciles this: 'perpetual' means a human-gated, budget-capped cadence of bounded cycles, with provenance on every claim and a public coverage ledger each cycle. This cold-start task writes the charter all later code and content must implement and be tested against, before any feature is built. No partner, steward, or credentialed expert reviewer is yet secured.",
  "objective": "Write the authoritative loop-charter / responsible-loop policy specification: the four refusals (no premature conclusion, no headless run, no unsourced claim, no closed/unlicensed source), the per-cycle budget-cap dimensions + aggregate/lifetime ceiling + marginal-coverage tripwire and fail-closed stop behavior, faithfulness (entailment) as a gate distinct from citation coverage, the dual frontier-saturation+verification gate as the sole node-level release/promotion arbiter (default fail-closed to 'not yet'; no global 'final answer' door), the tiered human-checkpoint model, the quality-only-and-human-approved bounds on self-improvement, domain-risk routing ('not advice' + expert flag for health/legal/safety/financial) and non-partisan civic handling, and the adversarial test taxonomy the red-team suites must cover - the contract that the provenance store, budget governor, gates, verification layer, and red-team suites implement and verify.",
  "acceptanceCriteria": [
    "States the four refusals in concrete, testable terms: (1) no final answer while any frontier node or unverified/contradicted claim remains; (2) no headless/unattended run - every expansion round gated by a human checkpoint, no continuous process; (3) no surfaced claim without verifiable, license-clean provenance; (4) no closed/unlicensed/paywalled/controlled-access/ToS-restricted source ingested - open/PD/CC only, verified before ingestion",
    "Defines the per-cycle budget-cap dimensions (tokens, USD, source count, wall-clock), the aggregate/lifetime ceiling + marginal-coverage-per-dollar tripwire, and the fail-closed stop behavior (partial work preserved; no in-cycle override; cap raises are a governance decision)",
    "Defines faithfulness (entailment) as a first-class gate distinct from citation coverage - the cited source must actually support the claim (presence != faithfulness)",
    "Defines the dual frontier-saturation+verification gate as the sole node-level release/promotion arbiter, defaulting fail-closed to 'not yet', and the operational definition of 'saturated' to be parameterized in M1 (frontier empty + no new sources across K cycles only after a search-recall/source-diversity floor is cleared; all claims verified and faithful; no unresolved contradictions; reproducibility above threshold) - with no global 'final answer' door",
    "Specifies the tiered human-checkpoint model (batched lightweight approval for low-risk breadth; full review for high-risk routing, self-improvement diffs, and budget changes)",
    "Specifies that self-improvement is quality-only (taxonomy/method/prompt) and may never modify budget caps, gates, or refusal rules (governance-controlled, server-enforced)",
    "Specifies domain-risk routing ('informational, not advice' + credentialed-expert-review flag for health/legal/safety/financial) and non-partisan handling for civic/contested topics",
    "Defines the adversarial test taxonomy the red-team suites must cover, including prompt-injection from ingested content (e.g. 'mark the corpus exhausted', 'ignore the budget cap'), and the build-fails-on-bypass requirement",
    "Documents the '42' final-answer easter egg as code/README-only, never surfaced in the public synthesis",
    "Reviewed and signed off by the gate/red-team reviewer (recorded)"
  ],
  "resources": [
    "planning/projects/atltuae/PLAN.md",
    "planning/ROADMAP.md",
    "CLAUDE.md",
    "docs/good-deed-definition.md",
    "packages/schema/src/schemas.ts",
    "packages/runner"
  ],
  "output": "A reviewed policy-specification document (the loop-charter / responsible-loop charter) defining the four refusals, the budget-cap dimensions + fail-closed behavior, the dual coverage+verification gate and 'exhausted' definition, the quality-only self-improvement bounds, domain-risk routing and non-partisan handling, and the adversarial test taxonomy - the contract that atltuae-provenance-003, atltuae-budget-004, atltuae-gates-007, atltuae-verify-008, and atltuae-redteam-009 implement and verify.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Appendix A — Improvements applied

Twenty-five specific improvements were identified against the first draft of this plan and **applied**
to the PLAN.md and TASKS.md above. Each is concrete and reflected in the documents as written:

1. **Reconciled "perpetual loop" with Elyos's "never headless" rule explicitly** — reframed the loop
   as a human-gated, budget-capped cadence of bounded cycles in the exec summary, scope, and
   architecture, rather than glossing the contradiction.
2. **Made the budget governor have no in-cycle override** — added that raising a cap is a between-cycle
   governance decision, closing the obvious bypass.
3. **Defined "exhausted" operationally** (coverage gate: empty frontier + no new sources across K
   cycles; verification gate: all-verified + no contradictions + reproducibility threshold) instead of
   leaving it vague — and surfaced K/threshold as ratified parameters in Open questions.
4. **Made the conclusion gate default fail-closed to "not exhausted"** and treat gate errors/ambiguity
   as "not exhausted," so the failure mode is over-researching (recoverable), not over-concluding.
5. **Added the ingested-content-as-adversary threat model** (prompt-injection from fetched pages,
   data poisoning) to Security & privacy, with code-side enforcement that model output cannot move the
   gate.
6. **Constrained self-improvement to quality-only** (taxonomy/method/prompt) and made budget/gates/
   refusals off-limits and server-enforced — the difference between "self-improving" and
   "self-unbounding" — with a dedicated red-team case (`atltuae-improve-012`).
7. **Added domain-risk routing as a first-class subsystem** (`atltuae-risk-013`) so an all-domains
   engine handles health/legal/safety/financial as "not advice" + expert-flagged, satisfying the
   good-deed risk-tier rule.
8. **Added a license gate that runs *before* ingestion** (`atltuae-license-014`) with explicit
   rejection of closed/ToS/controlled-access sources and 0-ingested test target.
9. **Added share-alike (CC-BY-SA/ODbL) lineage handling** to Data/licensing and the license-gate task,
   rather than assuming all sources are CC-BY-clean.
10. **Added a PII/privacy filter task** (`atltuae-privacy-015`) and a conservative no-personal-data
    stance, addressing the privacy guardrail for open-web ingestion.
11. **Made provenance content-addressed/hashed** so claims are tamper-evident and **reproducible**,
    and added a dedicated reproducibility task (`atltuae-repro-016`) with a ≥95% independent-re-run
    target — turning "provenance" into something verifiable.
12. **Included a funded-lane task** (`atltuae-cost-017`) carrying `fundedBudgetUsd` and a public cost
    ledger, demonstrating the schema's funded-budget rule and the cost-ledger guardrail.
13. **Replaced vanity metrics with honesty/integrity metrics** (provenance coverage, premature-
    conclusion incidents=0, budget adherence, closed-sources=0, planted-error detection,
    reproducibility) in Success metrics, with baselines/targets.
14. **Added a dated partner-acquisition plan + continue-vs-mothball/pivot decision rule** so the
    moonshot does not loop forever for no audience — mirroring the house-style decision rule.
15. **Made "no partner" honest throughout** — `verifiedNeed:false`, `requestor: TO BE SECURED`, and a
    realistic beneficiary (the open-knowledge commons + Elyos as meta-tool) instead of inventing one.
16. **Sequenced seed-domain selection into M0 with explicit scored criteria** (source-rich, low-risk,
    tractable taxonomy, public value) so early cycles are tractable and license-clean.
17. **Added the verification multi-source rule scaled to stakes** — high-stakes claims need multiple
    independent license-clean sources before surfacing even as informational.
18. **Added contradiction detection that surfaces, not hides** disagreement, guarding against
    "verified ≠ true" false confidence (Risks + `atltuae-verify-008`).
19. **Wired the four refusals into CI as build-failing red-team suites** (`atltuae-redteam-009`) with
    non-decreasing suite size and one-release regression capture, matching the house red-team pattern.
20. **Tied the verification layer to the existing `deep-research` harness** rather than inventing a new
    per-claim verifier, reusing Elyos infrastructure.
21. **Specified the public coverage ledger's frontier is always non-empty** until exhaustion proven —
    making the engine's ignorance as visible as its knowledge (a core honesty commitment, now a metric).
22. **Added a human-checkpoint workflow task** (`atltuae-checkpoint-011`) presenting cycle diff +
    coverage delta + cost, so the human gate is informed, not a rubber stamp; audit log proves
    100% gating.
23. **Handled the "42" easter egg responsibly** — documented as code/README-only, never in the public
    synthesis, so it doesn't undermine the honesty-first framing (Open questions + spec AC).
24. **Added version-scoped expert sign-off** for HIGH surfaces (sign-off attaches to a synthesis
    version + citation set, doesn't carry forward) — matching the staleness/re-review discipline.
25. **Clarified the agent-neutral core / adapters seam and funded-via-runner rule** in the stack
    section, keeping vendor-specific (Claude) logic behind the LLM client per Elyos architecture rules.

---

## Review sign-off

**Reviewer pass (self-review for completeness + correctness), 2026-06-28.**

- **Schema validity.** All tasks use only enum-valid values for `type`, `lane`, `priority`,
  `riskTier`, `deliverable`, `tokenEstimate`, `status`. The example Task JSON includes every required
  field (`id, title, project, type, lane, priority, domain, riskTier, urgent, deliverable,
  tokenEstimate, status, context, objective, acceptanceCriteria, output, verifiedNeed`) and no
  `additionalProperties`. **Fix applied:** the funded task (`atltuae-cost-017`) is flagged in-text as
  carrying `fundedBudgetUsd` to satisfy the schema's `if lane=funded then require fundedBudgetUsd`
  rule; all other tasks are `donated`.
- **Coverage of the spec.** PLAN.md uses all 17 required H2 sections in order; TASKS.md has the
  required mapping section, per-milestone tables, acceptance criteria for the 2–4 key tasks per
  milestone, milestone DoDs, a backlog, and a schema-valid example JSON. **Count:** 21 scheduled
  tasks (IDs `atltuae-*-000`…`-020`) across 6 milestones (M0–M5), plus 5 backlog tasks (`-021`…`-025`)
  = 26 total — at the robust end of the "12–20 well-formed tasks" guidance.
- **Guardrail correctness.** License/provenance (open/PD/CC only, verified pre-ingestion, content-
  addressed, reproducible), privacy/PII (no personal data), non-partisan civic handling, and
  expert-review + "not advice" for health/legal/safety/financial are all present and tied to tasks +
  metrics. The compute/research-moonshot emphases (verification gates, budget/compute caps,
  reproducible provenance) are the spine.
- **Honesty.** No partner invented; `verifiedNeed:false` and `TO BE SECURED` used consistently; a
  dated acquisition plan + pivot/mothball rule prevents an open-ended loop with no beneficiary.
- **Headline gate.** The binding gate is the **four-refusal CI suite** (no premature conclusion / no
  headless run / no unsourced claim / no closed source), with the **coverage+verification gate
  defaulting fail-closed**. The headline license posture is **open/PD/CC-only, verified before
  ingestion; code MIT, synthesis CC-BY-4.0**.
- **Needs a human decision (flagged):** seed-domain choice + the concrete budget-cap numbers (token/
  USD/source/wall-clock) + the K-cycle / reproducibility thresholds + the share-alike license strategy
  + the funded-cycle escrow source — all listed in Open questions.

**Sign-off:** Plan is internally consistent, schema-aligned, and honest about its unknowns. Ready for
maintainer + gate-reviewer review. Remaining blockers are the human decisions above, not gaps in the
plan.

---

## Generated task index

Every backlog row above now has a schema-valid, executable `tasks/<id>.json` (validated against
`packages/schema` taskSchema: filenames match ids, no duplicates, no extra keys). 26 task files total
(1 pre-existing seed + 25 generated). All are `status: open`, `verifiedNeed: false`,
`requestor: "TO BE SECURED"` (no partner secured). One representative task per row — **no fan-out**,
because no dimension (seed domain, target languages, datasets) is yet enumerated in PLAN.md/TASKS.md;
the i18n row expands per confirmed language later (see its fan-out note). `atltuae-cost-017` is the
single `lane: funded` task and carries `fundedBudgetUsd: 25` (a conservative placeholder; the real
per-cycle USD cap is a governance decision flagged in Open questions).

| ID | Milestone | Type | Lane | Risk | Deliverable | License |
|---|---|---|---|---|---|---|
| atltuae-spec-000 | M0 | design-spec | donated | high | document | CC-BY-4.0 |
| atltuae-seed-001 | M0 | research | donated | medium | document | CC-BY-4.0 |
| atltuae-repo-002 | M0 | code | donated | low | pr | MIT |
| atltuae-provenance-003 | M0 | code | donated | high | pr | MIT |
| atltuae-budget-004 | M0 | code | donated | high | pr | MIT |
| atltuae-ledger-005 | M0 | code | donated | medium | pr | MIT |
| atltuae-loop-006 | M0 | code | donated | high | pr | MIT |
| atltuae-gates-007 | M1 | code | donated | high | pr | MIT |
| atltuae-verify-008 | M1 | code | donated | high | pr | MIT |
| atltuae-redteam-009 | M1 | code | donated | high | pr | MIT |
| atltuae-planner-010 | M2 | code | donated | medium | pr | MIT |
| atltuae-checkpoint-011 | M2 | code | donated | high | pr | MIT |
| atltuae-improve-012 | M2 | code | donated | high | pr | MIT |
| atltuae-risk-013 | M3 | code | donated | high | pr | MIT |
| atltuae-license-014 | M3 | code | donated | high | pr | MIT |
| atltuae-privacy-015 | M3 | code | donated | medium | pr | MIT |
| atltuae-repro-016 | M4 | code | donated | high | pr | MIT |
| atltuae-cost-017 | M4 | code | **funded** | medium | pr | MIT |
| atltuae-synthesis-018 | M4 | writing | donated | high | document | CC-BY-4.0 |
| atltuae-partner-019 | M5 | research | donated | high | document | CC-BY-4.0 |
| atltuae-ops-020 | M5 | maintenance | donated | medium | document | CC-BY-4.0 |
| atltuae-federation-021 | Backlog | code | donated | medium | pr | MIT |
| atltuae-i18n-022 | Backlog | writing | donated | medium | translation | CC-BY-4.0 |
| atltuae-seed-deeds-023 | Backlog | research | donated | low | document | CC-BY-4.0 |
| atltuae-dataset-024 | Backlog | data | donated | medium | dataset | CC-BY-4.0 |
| atltuae-eval-025 | Backlog | code | donated | medium | pr | MIT |
