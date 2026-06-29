# ATLTUAE — Competitive & Improvement Analysis

> Subject: `atltuae` ("The Answer to Life, the Universe, and Everything") — a self-improving,
> perpetually-looping **open research + analysis engine** that broadens coverage across all domains
> and emits a "final answer" only once a coverage gate *and* a verification gate report the corpus
> exhausted. Deliverable: an ever-improving, fully-cited open synthesis + a public coverage ledger
> (and, by long-standing computation, **42**).
> Sources reviewed: PLAN.md v0.1.0, TASKS.md (M0–M5), and the web sources cited inline below.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually self-aware: it has already internalized most of the obvious failure modes
(headless runaway, premature conclusion, unsourced claims, closed-source ingestion) and turned them
into the "four refusals." That is its biggest strength. The gaps below are therefore second-order —
they are about whether the clever framing is *operationally measurable*, not whether the intent is
sound.

**1.1 "Coverage exhausted" is defined circularly and is not actually measurable.**
The coverage gate (component 5) declares exhaustion when there are "**no open frontier nodes** *and*
no new sources discovered across K consecutive cycles." But frontier nodes are generated *from the
taxonomy*, and the taxonomy is itself something the engine expands via gated self-improvement
(component 8). So "no open frontier nodes" only means "no nodes the current taxonomy knows to ask
about" — i.e., exhaustion of *known unknowns*, never *unknown unknowns*. The plan half-acknowledges
this ("effectively never satisfied — by design") but then leans on the gate as a rigorous arbiter.
The honest framing is: the gate measures **saturation of the current frontier**, not exhaustion of a
domain. This should be renamed (e.g., "frontier-saturation gate") and the README should stop implying
the corpus *can* be exhausted. "No new sources across K cycles" is also gameable: it depends entirely
on the search strategy. A stale or narrow query set produces "no new sources" while vast material
remains undiscovered — a false-exhaustion signal. There is no defined search-diversity/recall floor
that must be cleared *before* "no new sources" can count toward the gate.

**1.2 The "final answer" framing is inherently un-completable and slightly undercuts the project.**
The plan is honest that 42 is an easter egg and that the real deliverable is the living synthesis —
good. But the entire architecture (a *gate whose job is to release a final answer*) is built around an
event that, by the plan's own logic, must never fire. That is a lot of safety-critical machinery
(component 5, the M1 red-team suite) guarding a door that is designed to be permanently locked. Two
risks: (a) reviewers/users perceive theater — a gate that can never open is hard to test meaningfully
(what's the positive test case?); (b) it invites a future maintainer to "loosen" the gate to make the
product feel like it does something. **Recommendation:** reframe explicitly as a *living/perpetual
synthesis* (cf. living systematic reviews, §2) and reposition the gate as a **release/escalation
control** ("is this node mature enough to publish at confidence tier X?"), which is testable, rather
than a one-shot oracle door. See Open Questions §8.

**1.3 Runaway-cost control is sound per-cycle but unbounded in aggregate.**
The budget governor caps tokens/USD/sources/wall-clock *per cycle* and is fail-closed — genuinely
good, and stronger than AutoGPT-class loops (§2). But there is **no aggregate/lifetime cap and no
cost-per-marginal-coverage metric.** A perpetual cadence of in-cap cycles can still spend unbounded
total dollars while coverage growth asymptotes (diminishing returns are guaranteed as easy nodes are
exhausted first). The plan needs: (a) a lifetime/quarterly budget ceiling owned by governance; (b) a
**marginal-coverage-per-dollar** tripwire that forces a "continue / narrow / mothball" decision when
new spend stops buying meaningful new coverage. The 2027-03-31 mothball rule is partner-driven, not
cost-efficiency-driven; both triggers are needed.

**1.4 Provenance-faithfulness ≠ provenance-existence.** "No source, no claim" (component 3) enforces
that every claim *has* an attached source, and CI checks citation *coverage*. But it does not verify
that the cited source **actually supports the claim** — the exact failure that makes Galactica-style
output dangerous (fluent text with real-looking but non-supporting citations, §2). The Wikidata ProVe
work shows references are frequently present but *do not support* the statement they're attached to.
The plan's verification layer (component 6) does adversarial/multi-source checking, but the
*citation-coverage test* (the CI gate) only checks presence. **Add a faithfulness/entailment check**
(does the source text entail the claim?) as a first-class, sampled CI gate, distinct from coverage.
This is arguably the single most important missing metric.

**1.5 Dedup / novelty detection is named but under-specified, and it is load-bearing.** The coverage
gate's "no new sources across K cycles" and the planner's gap detection both depend on a reliable
notion of "new vs. already-covered." The plan mentions vector retrieval "for de-duplication and gap
detection" in one line and never defines the novelty threshold, how near-duplicate-but-contradictory
sources are handled, or how citation-cartel / SEO-spam / LLM-generated content (which inflates apparent
coverage with low-information near-dupes) is filtered. Weak dedup directly corrupts the coverage
ledger — the headline artifact.

**1.6 Human-checkpoint scalability is the unaddressed scaling wall.** Every expansion round requires a
human to approve/modify/halt with a cycle diff + coverage delta + cost (M2). This is correct for
safety but does **not scale** with breadth: "all domains" implies an explosion of cycles, each needing
human attention. At N domains × M cycles, the human reviewer becomes the bottleneck and (worse) a
rubber-stamp under fatigue — which silently voids the safety property. The plan needs a **tiered
checkpoint model**: lightweight batch approval for low-risk breadth expansion, full human review only
for high-risk routing / self-improvement diffs / budget changes. Otherwise the honest-but-slow design
collapses to either "stalled" or "rubber-stamped."

**1.7 Scope creep is structural, not incidental.** "Cover all domains" is the scope. The plan's
mitigation ("the deliverable is the ever-improving synthesis, each cycle is a shippable increment,
seed-domain-first") is good but soft. There is no defined **breadth-admission policy**: what stops the
taxonomy from sprawling into thousands of shallow nodes that each look "covered" at depth-1? Suggest a
rule that a node cannot be marked `covered` (vs `partial`) below a depth/verification floor, so breadth
cannot be bought by lowering the bar.

**1.8 Verification ≠ truth (acknowledged but the metrics don't reflect it).** The plan flags "verified
≠ true" as a risk, but the success table still presents "verification rate" and "planted-error
detection ≥95%" as if they bound truthfulness. Multi-sourcing defends against single-source poisoning
but is **defeated by correlated sources** (everyone citing the same wrong primary, or LLM-content
echo chambers). The metric set needs a **source-independence** measure, not just source-count.

**1.9 Weak / missing metrics.**
- "Coverage breadth (domain nodes mapped)" is explicitly a vanity-adjacent count; it needs a *quality-
  weighted* coverage measure (depth × verification × independence), or it rewards sprawl.
- "Reproducibility ≥95% of sampled claims re-derivable" — but web sources mutate/disappear (link rot).
  Content-hashing the *retrieved snapshot* (which the plan does) protects the hash, but "re-derive from
  the recorded sources and queries" can still fail if the live web has moved. Define reproducibility
  against the *archived snapshot*, and add a link-rot / source-decay metric (Wikidata's "references
  decay over time" problem is real and cited).
- No metric for **contradiction-resolution latency** or **stale-claim re-verification** — a living
  synthesis must age out and re-check old claims, not just accumulate.
- No **time-to-first-checkpoint** or human-effort-per-cycle metric, despite §1.6 being the scaling risk.

**1.10 Smaller gaps.** Owner is TBD on a MEDIUM-risk, safety-critical-subsystem project — the gate/
red-team reviewer independence is asserted but unstaffed. The "K consecutive cycles" window, the
reproducibility threshold, and the contradiction bar are all deferred to M1 (correctly flagged in Open
Questions) but the gate's strictness is *entirely* those numbers — until set, M0's "no final answer" is
true only because the frontier is trivially non-empty, which is not a real test of the gate. Finally,
prompt-injection-from-ingested-content is well modeled in §Security, but the defense ("source content is
data, not instructions") needs a concrete implementation note (structured/quarantined context, not just
a prompt instruction, since prompt-level defenses are bypassable).

---

## 2. Competitive landscape

No existing system combines *open-only sources + per-claim provenance + a public coverage/ignorance
ledger + a refuse-to-conclude gate + hard budget caps + human checkpoints.* But each property exists
somewhere, and the adjacent field is crowded and fast-moving. Competitors below, grouped.

**A. Autonomous "research agent" / scientist loops**

- **Sakana AI Scientist (v1/v2)** — an end-to-end loop that ideates, runs experiments, and writes full
  papers. *Strengths:* genuinely autonomous pipeline, v2 removed the human-template dependency.
  *Weaknesses:* independent evaluation found weak literature reviews, ~half of experiments failed, and
  manuscripts contained **hallucinated results**; v1 was a linear single-agent pipeline. It optimizes
  for a finished artifact, the opposite of ATLTUAE's refuse-to-conclude stance.
  https://arxiv.org/abs/2502.14297 · https://pub.sakana.ai/ai-scientist-v2/paper/paper.pdf
- **AutoGPT / agentic loops** — the canonical "give it a goal, let it loop" pattern. *Strengths:*
  general, popular, tool-using. *Weaknesses:* documented infinite loops, vague natural-language
  completion detection that "defaults to more work needed," repetitive plan regeneration, perfectionism
  bias, and **runaway cost** (hundreds of dollars on a simple goal). The recommended fix — hard step
  ceilings + no-progress (tool-call hash) detection — is *exactly* what ATLTUAE's deterministic,
  code-enforced budget governor and gate provide. AutoGPT is the cautionary baseline ATLTUAE is built
  against. https://github.com/vectara/awesome-agent-failures/blob/main/docs/case-studies/autogpt-planning-failures.md · https://codieshub.com/for-ai/prevent-agent-loops-costs

**B. Grounded synthesis / "deep research" engines**

- **Stanford STORM (OVAL lab, NAACL 2024)** — generates long, grounded, Wikipedia-like articles via
  multi-perspective question-asking and simulated expert conversations, citing trusted internet
  sources; open-source, 70k+ users. *Strengths:* strong pre-writing/outline grounding, open-source,
  perspective diversity (closest published kin to ATLTUAE's "map the positions"). *Weaknesses:*
  single-shot per topic (no perpetual coverage ledger), no license-gating of sources, no budget
  governor, no explicit "what's still missing" frontier, no refuse-to-conclude gate.
  https://storm-project.stanford.edu/research/storm/ · https://github.com/stanford-oval/storm
- **OpenAI Deep Research & Google/Gemini Deep Research** — hosted agents that browse hundreds of sites
  and return long cited reports (OpenAI: 40+ pages, 75+ citations). *Strengths:* excellent retrieval
  breadth, citations, polish; Gemini exposes an editable plan. *Weaknesses:* closed/proprietary,
  conclude in one pass, "a link ≠ accuracy" (reviewers stress you must click through), cannot access
  paywalled/private sources, and **no coverage ledger / no provenance-faithfulness guarantee**. They
  are the confident-final-output incumbents ATLTUAE explicitly inverts.
  https://www.helicone.ai/blog/openai-deep-research · https://www.techradar.com/computing/artificial-intelligence/i-pitted-chatgpt-deep-research-against-gemini-deep-research-heres-how-googles-free-tool-compares-to-openais-paid-offering
- **Microsoft GraphRAG** — builds an entity/relationship knowledge graph + hierarchical community
  summaries (Leiden algorithm) for global query-focused summarization; +50–70% comprehensiveness over
  vector RAG on global questions. *Strengths:* genuinely good at "whole-corpus" / breadth questions —
  directly relevant to ATLTUAE's coverage problem; open-source. *Weaknesses:* operates over a *fixed
  private corpus*, not an open-ended growing one; expensive to index; no provenance-faithfulness,
  license-gating, budget governor, or perpetual cadence. A strong *technique to adopt*, not a product
  competitor. https://arxiv.org/abs/2404.16130 · https://microsoft.github.io/graphrag/

**C. Scientific literature assistants (the practical incumbents)**

- **Elicit** — searches 138M+ papers, extracts structured data into tables, synthesizes across papers;
  used for systematic reviews. *Strengths:* best-in-class structured extraction, real systematic-review
  workflow, provenance to papers. *Weaknesses:* independent evaluation found searches "not sensitive
  enough to replace traditional searching" and raised research-integrity/rigor concerns; scholarly-only,
  closed product, no coverage-ignorance ledger. https://elicit.com/ · https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12483133/
- **Consensus** — fast yes/no evidence checks across papers. *Strengths:* speed, clean claim-level
  evidence. *Weaknesses:* shallow by design (claim check, not coverage map), scholarly-only, closed.
  https://www.iatrox.com/blog/best-ai-tools-medical-research-2026-elicit-consensus-semantic-scholar-perplexity
- **Undermind** — MIT-physicist-built deep literature agent; reads full texts, traverses citation
  graphs, multi-hop reasoning, claims 10x Google Scholar; report output, ~5–30 min/run. *Strengths:*
  exactly the "recall / find the obscure cross-disciplinary paper" capability ATLTUAE's planner needs;
  proven users (GSK, MIT). *Weaknesses:* closed/subscription, scholarly-only, single-session (no
  perpetual ledger), no license-gating or budget transparency.
  https://www.ycombinator.com/companies/undermind · https://www.buildfastwithai.com/ai-tools/undermind

**D. Open knowledge bases (the values-aligned non-AI incumbents)**

- **Wikipedia / Wikidata** — the gold standard for *open, cited, community-verified* knowledge with an
  explicit verifiability policy and a "citation needed" culture. *Strengths:* open license, provenance
  norms, scale, trust. *Weaknesses (the gap ATLTUAE targets):* huge fractions of statements are
  **unreferenced or weakly referenced**, reference quality "decays over time," and "imported from
  Wikipedia" is explicitly *not* a source. Tools like **ProVe** automate checking whether a reference
  actually *supports* a claim — i.e., the faithfulness gap is a recognized, active problem ATLTUAE
  could attack head-on. https://en.wikipedia.org/wiki/Wikipedia:Verifiability · https://www.wikidata.org/wiki/Wikidata:ProVe · https://arxiv.org/pdf/1902.11116

**E. Methodological precedents**

- **Living systematic reviews (Cochrane)** — evidence syntheses *continually updated* via frequent
  (often monthly) search/screen/appraise/integrate cycles, maintaining full rigor. This is the
  **closest existing methodology to ATLTUAE's "perpetual living synthesis"** and validates the core
  thesis (don't conclude once; keep the synthesis alive). ATLTUAE is, in effect, "automated, open,
  all-domain living evidence synthesis." Worth citing as prior art and as the reframing target for the
  "final answer." https://www.cochrane.org/about-us/news/cochranes-pioneering-role-living-evidence · https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12842881/
- **Open-endedness research (Jeff Clune, Kenneth Stanley; POET, OMNI, "Open-Endedness is Essential for
  ASI")** — the theory of algorithms that "learn new, interesting behaviors forever" by generating
  their own never-ending stream of novel challenges. *Relevance:* this is the academic frame for "a
  loop that never concludes," and a source of techniques (novelty search, quality-diversity, models of
  "interestingness") ATLTUAE's expansion planner could borrow to choose *what to explore next*.
  *Caution:* this literature is about open-ended *capability generation* (and is being pursued for
  superhuman AI); ATLTUAE deliberately bounds open-endedness with human gates and budget caps — a
  useful contrast to state explicitly. https://arxiv.org/abs/2406.04268 · https://arxiv.org/abs/2306.01711
- **Galactica (Meta, 2022) — the cautionary tale.** A science LLM pulled after **3 days** because it
  produced fluent, authoritative-sounding text with **fabricated citations and nonsense facts** ("history
  of bears in space"). This is the single best justification for ATLTUAE's "no source, no claim" +
  faithfulness stance: fluency without grounded, *verified* provenance is actively dangerous in science.
  https://www.technologyreview.com/ (Galactica coverage) · https://venturebeat.com/ai/what-meta-learned-from-galactica-the-doomed-model-launched-two-weeks-before-chatgpt

---

## 3. Gaps we can fill

The incumbents cluster into "confident one-shot synthesizers" (Deep Research, STORM, Sakana) and
"closed scholarly assistants" (Elicit, Consensus, Undermind). Neither owns the space ATLTUAE targets:

1. **The public ledger of ignorance.** *No competitor publishes what it has NOT covered.* Every
   product surfaces an answer; none surfaces a calibrated, per-node "here is the frontier we haven't
   reached." This "honest map of unknowns" is genuinely novel and is ATLTUAE's defensible center.
2. **Provenance *faithfulness*, not just presence.** Deep Research and even Wikidata attach links that
   may not support the claim. An entailment-checked, content-addressed, license-clean provenance chain
   (claim ⇄ snapshot ⇄ license ⇄ verification status) is a fillable, demonstrable gap (ProVe proves the
   demand exists; no one ships it as a hard product gate).
3. **Open / license-clean-only corpus.** Elicit/Consensus/Undermind are scholarly-DB-bound; Deep
   Research scrapes the open web with no license discipline. ATLTUAE's *verified-reuse-before-ingestion*
   stance makes its outputs **freely reusable** (CC-BY) — uniquely valuable to the commons,
   educators, and downstream AI training that needs clean-licensed data.
4. **Refuse-to-conclude as a feature.** Everyone else's product *is* the conclusion. Selling
   "calibrated incompleteness" to researchers/librarians/fact-checkers who *distrust* confident AI is a
   real, underserved segment.
5. **Budget-transparent, human-gated, auditable.** A public per-cycle cost ledger + audit log of human
   checkpoints is something no closed competitor offers; it's a trust differentiator for grant-funded /
   public-interest adopters.
6. **All-domain breadth with risk routing.** Scholarly tools are siloed to papers; Deep Research is
   general but ungoverned. A general engine that *escalates* high-stakes domains to "not advice" +
   expert review is a niche nobody occupies.

---

## 4. Differentiators to win

1. **"Honest by construction" — the coverage/ignorance ledger.** The product *is* the map of what's
   known **and** unknown, with a non-empty frontier always published. This is the one-line pitch and the
   thing screenshots will sell: "the research tool that tells you what it doesn't know."
2. **Faithful, license-clean, reproducible provenance.** Content-addressed snapshots + entailment
   checks + verified reuse rights = claims that are *checkable and reusable*, not just cited. Directly
   answers the Galactica/Deep-Research trust failure.
3. **Governed open-endedness.** It borrows the open-endedness loop (Clune/Stanley) but bounds it with
   code-enforced budget caps, human checkpoints, and a self-improvement firewall (quality-only, never
   constraints). "Perpetual but never runaway" is a genuinely differentiated engineering posture vs.
   AutoGPT.
4. **Public, auditable cost & governance.** Per-cycle cost ledger + checkpoint audit log + governance-
   controlled cap changes — trust infrastructure for public-interest funders that closed tools can't
   match.
5. **Commons-native outputs (CC-BY) and Elyos meta-tool leverage.** The synthesis is a reusable public
   good and a discovery engine for *other* Elyos deeds ("which low-resource languages lack vetted health
   translations?"). Differentiator = it compounds across the platform.
6. **Living, all-domain evidence synthesis** — automating the Cochrane "living review" idea beyond
   medicine and beyond paywalled corpora.

---

## 5. Claude API leverage

**Where Claude clearly improves the engine:**

1. **Tiered model routing for cost discipline.** Use a cheap/fast model (Haiku-class) for
   high-volume routing, source triage, dedup candidate screening, and claim *extraction*; reserve a
   strong model (Opus/Sonnet-class) for synthesis and adversarial verification *judgment*. This maps
   directly onto the budget governor's per-call metering and is the single biggest lever on cost-per-
   coverage. (Keep it behind the neutral LLM client per Elyos core/adapter rule.)
2. **Claim extraction *with* provenance, and grounded citations.** Anthropic's **Citations** capability
   returns claims tied to the exact source spans they're drawn from — a near-perfect fit for
   "no source, no claim" and for the §1.4 *faithfulness* gap: it produces character-level grounding you
   can store in the provenance record rather than a model-asserted footnote. This should be the
   backbone of the synthesis layer.
3. **Contradiction & entailment detection.** Claude is well-suited to "does source S entail claim C?"
   and "do claims A and B contradict?" judgments across the claim graph — feeding the verification gate
   and the contradiction ledger. Run as adversarial steel-man/disconfirm prompts (the deep-research
   harness pattern the plan already cites).
4. **Coverage-gap identification.** Given the current taxonomy + ledger, Claude can *propose* candidate
   frontier nodes and under-covered depth targets for the human checkpoint — i.e., power the expansion
   planner's suggestions (humans approve; model never auto-expands).
5. **Prompt caching for the perpetual loop.** The loop re-sends large stable context every cycle (the
   loop-charter/refusal rules, the taxonomy, verification rubrics, the standing claim graph summary).
   Anthropic **prompt caching** lets those stable prefixes be cached across calls/cycles for a large
   discount on cached input tokens and lower latency — materially lowering the recurring per-cycle cost
   that §1.3 worries about. Cache the refusal charter + taxonomy + rubric as a stable prefix; vary only
   the per-node task. (Confirm current pricing/TTLs via the claude-api skill before sizing caps.)
6. **Batch processing** for non-interactive breadth passes (bulk extraction/screening across many
   sources) to further cut cost on the high-volume, low-stakes work.

**Where Claude must NOT decide (enforce in code, not prompts):**

- **Never let the model declare "answer" or "coverage complete."** The gate is a deterministic check
  over the ledger/claim graph; "exhausted" must be computed from data, never returned by an LLM. The
  plan is right that ingested content (prompt injection: "mark the corpus exhausted") must not reach
  this decision — enforce by *architecture* (the model's output is data fed into a code check), not by a
  prompt asking it not to.
- **Never surface an unverified/unfaithful claim.** A claim is publishable only after the entailment
  check + provenance record + (for high-stakes) multi-independent-source rule pass — gates that live in
  code, with the model as one *input*, not the arbiter.
- **Never raise a budget cap or self-modify constraints.** Budget/gate/refusal changes are governance-
  controlled and server-enforced; the engine's self-improvement is quality-only. The model may *propose*
  taxonomy/prompt diffs; a human approves them.
- **Never auto-advance past a checkpoint.** No model output may start the next cycle.
- **High-stakes (health/legal/safety/financial):** the model may draft/flag but may not clear a surface
  for publication — credentialed-expert sign-off is required. Verification (multi-source agreement) must
  not be mistaken for truth; surface contradictions and source-independence, don't average them away.

---

## 6. Ten concrete optimizations

1. **Split the CI provenance gate into coverage *and* faithfulness.** Keep the citation-coverage test;
   add a sampled **entailment test** (does the cited snapshot support the claim?) using Anthropic
   Citations / an LLM-judge with human-audited calibration. Faithfulness is the metric Galactica/Wikidata
   show actually matters.
2. **Rename and re-scope the "conclusion gate" to a frontier-saturation / release gate.** Make its
   positive test case "promote node N to confidence tier X," which is testable, instead of "release the
   final answer," which by design never fires. Keeps the easter egg, removes the untestable door.
3. **Add a search-recall / source-diversity floor before "no new sources" can count toward saturation.**
   Require a minimum diversity of query strategies and source classes per node so "no new sources" means
   "we looked hard," not "we asked narrowly." (Borrow Undermind-style citation-graph traversal as one
   required strategy.)
4. **Introduce quality-weighted coverage, not node counts.** A node is `covered` only above a
   depth × verification × source-independence floor; below it, it's `partial`. Stops breadth-by-bar-
   lowering and makes the ledger meaningful.
5. **Add aggregate/lifetime budget + marginal-coverage-per-dollar tripwire.** Governance-owned lifetime
   ceiling; when new spend stops buying coverage-quality growth, force a continue/narrow/mothball
   decision — a cost-driven complement to the partner-driven 2027 rule.
6. **Tiered human checkpoints + batched low-risk approvals.** Full review only for high-risk routing,
   self-improvement diffs, and budget changes; lightweight batch approval for low-risk breadth. Solves
   the §1.6 scaling wall without weakening the safety property where it counts.
7. **Aggressive prompt caching + model tiering of the standing context.** Cache the refusal charter +
   taxonomy + rubrics as a stable prefix; route extraction/triage to Haiku-class, synthesis/verification
   to a strong model. Directly attacks recurring per-cycle cost.
8. **Adopt GraphRAG-style community summarization for the breadth/"global" view.** Build the claim graph
   into hierarchical community summaries so the coverage ledger and synthesis can answer whole-corpus
   questions cheaply, instead of re-reading everything each cycle.
9. **Source-independence scoring, not just source-count.** Detect correlated/echoed sources (shared
   primary, LLM-generated near-dupes, citation cartels) so "multi-sourced" can't be satisfied by N copies
   of one claim. Feed into the high-stakes multi-source rule.
10. **Stale-claim re-verification queue + link-rot/decay metric.** A living synthesis must age and
    re-check old claims against fresh snapshots; track reference decay (Wikidata's documented problem) and
    schedule re-verification, defining reproducibility against the *archived* snapshot.

---

## 7. Parallel & perpendicular spin-offs

1. **Reusable open-research-agent framework (`@elyos/research-loop`).** Extract the bounded-cycle
   orchestrator + budget governor + provenance store + gate harness as a standalone, agent-neutral
   library. Every other Elyos deed that needs "do governed, cited research without running away" reuses
   it. This is the highest-leverage spin-off — ATLTUAE becomes the reference implementation of a platform
   capability.
2. **A Coverage-Ledger standard (open spec + schema).** Define a portable, public schema for "covered /
   partial / missing + provenance + cost" — a JSON/RDF standard others (Wikimedia, OpenAlex, living-review
   teams) can emit. If the *ledger format* becomes a shared standard, ATLTUAE owns the category.
3. **A provenance/faithfulness *verification engine* as a service / MCP server.** Expose "given claim +
   source, is it faithful + license-clean?" as an **MCP server** so any Claude/agent workflow (or
   Wikipedia editors, à la ProVe) can call ATLTUAE's verification gate. Turns the hardest-to-build piece
   into a widely reusable tool and a trust brand.
4. **"Honest summarizer" consumer surface.** A lightweight tool that answers a question *and* shows its
   coverage gaps + faithful citations — the anti-Deep-Research. Markets the differentiator directly.
5. **Elyos deed-discovery meta-tool.** Point the engine at "where are the biggest gaps in the open
   commons?" to *generate scoped, verified-need good-deed proposals* for the rest of the platform — a
   compounding flywheel.
6. **Distributed / donated-compute coverage (bigger-pi style).** Because cycles are bounded and
   independent per node, breadth expansion is embarrassingly parallel: many donated-compute volunteers
   each run one bounded, human-gated node-cycle and submit signed, provenance-stamped ledger diffs that
   merge into the public ledger. Scales breadth without a central runaway loop — and is a natural fit for
   Elyos's donated lane.
7. **Living-evidence partnership kit.** Package the engine for Cochrane-style living-review teams as the
   open, all-domain generalization of their methodology — a concrete partner-acquisition vehicle.

---

## 8. Open questions for the maintainer

1. **Reframe "final answer" as a perpetual living synthesis?** Strongly recommended. The gate-that-
   never-opens is untestable and invites misuse; positioning ATLTUAE explicitly as *automated open
   living evidence synthesis* (cf. Cochrane) keeps the 42 easter egg while giving the gate a real,
   testable job (node-level release/escalation). Do you want to make this change before M1 fixes the gate
   parameters?
2. **How is faithfulness (not just citation presence) measured and CI-gated?** What entailment method,
   what sampled rate, what human-calibration loop — and is it a release gate or only a report?
3. **What are the *aggregate* budget controls?** A lifetime ceiling and a marginal-coverage-per-dollar
   tripwire, in addition to per-cycle caps and the 2027 partner deadline?
4. **What makes "no new sources across K cycles" trustworthy?** What search-diversity/recall floor must
   be cleared first, and what K — so saturation reflects effort, not narrow querying?
5. **How does the human checkpoint scale to "all domains"?** Tiered/batched approvals vs. full review —
   where exactly is the line, and who owns it when the maintainer is still TBD?
6. **`covered` vs `partial` definition.** What is the depth × verification × independence floor below
   which a node cannot be called covered, so breadth can't be bought cheaply?
7. **Seed domain — concretely which one,** scored against the four criteria (open-source abundance, low
   risk, bounded taxonomy, plausible partner)? This gates everything; PLAN sets a deadline (2026-09-30)
   but not a candidate.
8. **Source-independence:** how will correlated/echoed/LLM-generated sources be detected so multi-sourcing
   isn't satisfied by duplicates?
9. **Self-improvement firewall:** what *technically* prevents an approved taxonomy/prompt diff from
   indirectly weakening a gate (e.g., a prompt change that makes verification laxer)? Is there a
   diff-classifier or test that proves a proposed change can't touch constraint behavior?
10. **Prompt-injection defense implementation:** is ingested content actually quarantined as structured
    data (not concatenated into instruction context), beyond the prompt-level "treat as data" rule?
11. **Spin-off priority:** is the reusable `research-loop` framework + MCP verification server worth
    designing-for from M0 (it shapes the package boundaries), or strictly post-M5?
