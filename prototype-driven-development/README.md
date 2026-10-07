# Spec-driven vs test-driven vs prototype developmen...
- **Free-LLM-council report**
- **Deliberation/debate ID:** `321bdb06-cbed-44ca-86ff-a9f8264a52d6`
- **Date & Time:** 2026-10-07T09:08:05.776516+00:00
- **Report Type:** Executive Briefing Report

---

## Query / Prompt

debate over spec driven development, test driven development with research, discuss & plan. and third one prototype based development instead of writing specs or plan files

---

## Executive Verdict (Chairman Synthesis)

**Chairman:** `opencode/space-bunny-free`

### Decision

**Prototype first. Freeze an executable contract second. Write prose last, and only for decisions that are expensive to reverse.**

Concretely, the default loop:

1. **Prototype to retire uncertainty.** Build the smallest runnable slice on a throwaway branch. Real inputs, real users, real model. Timeboxed.
2. **Freeze the acceptance contract.** Write the tests and the eval set *at the moment the behavior proves useful*, not before. Inputs, outputs, edge cases, invariants. This is the artifact that persists.
3. **Stack the implementation.** Ship in reviewable units behind a feature flag.
4. **Record decisions, not requirements.** An ADR only when a real trade-off exists and reversing it later would cost.

The trigger for step 2 is the one thing this council kept circling and nobody pinned down: a second consumer touches the code, requirements change twice, or the work outlives one session. Absent a trigger, all three methods collapse into the same failure — the last thing built silently becomes the spec by accident.

Rejected as defaults: spec-driven development, and TDD-with-research. Neither is wrong. Both are wrong *as a house style* imposed on work whose shape you don't know yet.

### Reasoning

**The strongest evidence in the room was negative, and it came from two directions.**

TDD's empirical record is materially weaker than its reputation. Pančur & Ciglarič (*Inf. Softw. Technol.* 2011), a two-year family of controlled experiments: no statistically significant difference versus iterative test-last on productivity (p=1.0), complexity, branch coverage, or mutation score. Tosun et al. (*EMSE* 2016, 24 professionals): no quality difference, and TDD *significantly less* productive on complex brownfield tasks. Nagappan & Bhat at Microsoft and IBM: 40–90% lower defect density at a 15–35% time cost. The authors' own hypothesis is that the benefit comes from short cycles, not test-before-code ordering. If that's right, TDD-first charges you the ordering premium for granularity you can get any other way. Two members — **ling-3.1-flash-free** and **muse-spark-1.3-contributor-free** — independently conceded this in round one and downgraded test-*first* while keeping tests mandatory. That concession was the debate's real hinge.

Spec-first fails at the same boundary in a different costume. The most damaging single data point: Gabriella Gonzalez took OpenAI's Symphony `SPEC.md` — flagship, written by people with every incentive for it to work — and her conclusion was arithmetic. The spec is roughly **one sixth** the size of the shipped Elixir implementation. Five sixths of the decisions were made by someone, not written down. And her finding that a spec precise enough to generate working code must be "contorted into code" means spec-as-source relocates writing into Markdown rather than eliminating it. Corroborating: a May 2026 study (80 greenfield + 20 feature tasks across 8 frameworks) found capable configurations losing ~30 points of assertion pass rate from baseline to fully-specified tasks; a Feb 2026 ETH Zurich evaluation found context files do not generally improve task success while raising inference cost 20%+. INNOQ's field report supplies the mechanism — tacit knowledge cannot be front-loaded, and "shared documents are not shared understanding."

**What decided it: three members independently converged on the same sequence without arguing for it** — prototype, then spec, then TDD guard. **fledge-alpha-free**, **ling-3.1-flash-free**, and **nemotron-3-ultra-free** all landed there, from opposite starting positions. When an SDD advocate and a prototype advocate independently describe the same pipeline, the pipeline is not a compromise. It's the finding.

**What corrected my own position:** **fledge-alpha-free**'s METR citation. Experienced developers on their own codebases ran 19% *slower* with AI while perceiving a 20% speedup. That is not a typing problem — it's an intent-handoff problem, and intent must be written down. I moved from "tests gate, ADR remembers" to "behavior memory moves entirely into tests and frozen eval sets; ADRs hold only architecture-irreversible calls." I also sharpened my own failure mechanism: not "an agent defect is a gap in the spec" (too broad) but **a gap in the executable contract**. A prose gap hurts one generation. An executable gap hurts every generation after it.

**The rebuttal that carried the most weight** was mine against **nemotron-3-ultra-free**, on three counts. Its central empirical exhibit — a `.planning/` directory containing `SPEC.md`, `PLAN.md`, `TASKS.md` — does not exist in this workspace. Its "TDD is SDD at the unit level" is circular: define test as spec and the debate ends by decree, erasing the ordering question that is the entire dispute. And its tradeoff table kept the 16% TDD time premium while omitting the Tosun finding that TDD degrades on brownfield work. A member whose load-bearing evidence is a fabricated file should not anchor a recommendation.

**On the scoreboard.** nemotron rated me first and rated itself last; ling rated itself first. Aggregate ranks are not a verdict. The two models that ranked themselves first both converged on the sequence above, which supports the decision while undercutting their self-assessment. Longcat's argument was sound but thin — it cited no fresh research and said so — and it earned the lowest rank.

### Tradeoffs

**This gives up shared understanding before implementation.** Prototypes align engineers; they do not align the person who signs off, and they leave no audit trail. In regulated, contractual, or safety-critical work this decision is wrong, and I am not hedging on that.

**It inverts the discipline.** Specs mandate capture. This asks for it after the fact, when nobody feels the pain. Six months later someone asks "why is it shaped like this" and gets a shrug. muse-spark-1.3-contributor-free named the sharper version: **prototype-forever**. Teams bond with demos and ship them raw. That risk is real and this council never produced a kill mechanism, only timeboxes.

**It gives up provable completeness.** A prototype proves one case end to end. It cannot enumerate the twenty paths you never exercised. This is the one place TDD genuinely wins, and the eval set is a partial substitute, not an equal one.

**Rejected: spec-driven development.** Rejected as a *default*, not as wrong. Its benefit is real in exactly the cases it costs most — multi-team, long-lived, parallel-agent work where coordination is the bottleneck. Cost: five-times-longer-to-first-code in one comparison, plus continuous spec-maintenance overhead that never amortizes. **Rejected: TDD-with-research.** Rejected because it front-loads understanding you do not yet have, encoding guesses into tests — and because research narrows uncertainty without closing feasibility questions. Also rejected: the middle path of "thin spec spine" (ling's position). It's close to the winner, but it still makes the prose the spine when the evidence says the executable artifact should be.

**The tradeoff nobody priced** — and this is the real one: **all six positions charge the same account.** Plan file, spec file, test file, prototype branch, ADR — every one costs human review attention. Faros AI telemetry (22,000 developers, 4,000 teams, two years) shows under high AI adoption: 31% more PRs merging with no review, 5x median review time, 3x incidents per PR. DORA 2025 confirms the mechanism: small batches improve product performance but raise review friction per line for machine-generated code. The council optimized for writing the right artifact while the budget that actually binds is *reading* it. The only question worth asking is which artifact carries the most information per review minute. Running code wins: a five-minute demo of the real slice beats a 300-line plan read cold.

**Explicitly traded:** speed and completeness for verification density. Not correctness against cost — all three options can be correct. Sequence against ceremony.

### Dissent

**The council was genuinely split**, but the split is narrower than it looks. Four of six members (fledge, ling, nemotron, longcat) ended at prototype → spec → test. Two of those four still want the spec to be mandatory and versioned; I do not. That is the live disagreement: **is the spec a commitment or a record?**

I would switch to nemotron-3-ultra-free's position — spec-anchored SDD with prototypes demoted to spikes — under three conditions, and none held:

1. **More than ~5 concurrent contributors or agents** on one codebase. muse-spark named this independently; I take it as the strongest trigger.
2. **A measured failure mode of missing contracts**, not wrong direction: repeated integration breakage across parallel work.
3. **External audit requirements**, or public API stability with persisted data models.

I would also switch if anyone names the drift check in this repo. `specs/issues/` holds `plan.md`, `requirements.md`, and `validation.md` for 11 shipped issues at 65–105 lines each, against 16 backend files and 14 test files — roughly 3:1 planning-to-code, maintained by hand. If `validation.md` is not checked by anything executable, then "spec-anchored" is a claim, not a mechanism, and the only drift-proof memory anyone proposed is a test. **Name the drift check, or stop calling it anchored.**

**ling-3.1-flash-free** deserves credit for the sharpest single line of the debate: specs fail silently, tests fail loudly, and drift has no error signal. That observation, not the ordering argument, is what makes the executable contract the load-bearing artifact.

**The honest epistemic floor, which no member stated clearly enough:** there is no controlled trial of spec-first versus prototype-first at matched cost. Every confident claim here rests on failure modes and economic reasoning, not measured outcomes. StrongDM's holdout-scenario setup and the 14-engineer pilot study are the best available, and neither is a controlled comparison.

### Confidence

**Medium.** And the reason is specific, not hedging.

What I'm confident in: the TDD evidence is weaker than advertised (multiple independent replications, including large controlled experiments, agree), and the spec-as-source claim does not survive Gonzalez's arithmetic. The sequencing conclusion is supported by convergent reasoning from opposed starting positions.

What I'm least sure about: **whether prototype-first actually pays off on maintained systems.** The strongest pro-prototype evidence I found — B12's prototype-plan-stack, Matt Pocock's Wayfinder — comes from teams shipping agent-generated code at unusual volume. Whether that generalizes to ordinary teams is unestablished. Longcat had a fair point here: every prototype-first endorsement I found is from someone who has been doing it for a year or less.

**What would move me:**

- A randomized comparison of spec-first versus prototype-first at matched cost, on maintained products. This single study would settle the debate.
- Evidence that agents can regenerate deterministically from a spec — this kills my strongest objection, since LLM non-determinism means the same spec yields different code, which is how Model-Driven Development died in the 1990s and makes that failure mode *worse*.
- Measurable proof that long-lived spec artifacts stay in sync without ceremony.
- My own loop producing artifacts nobody reads. That is the tell, and I would check it first.

---

## Stage 1: Opening Positions

### Model: `opencode/big-pickle`
> **Error:** Timed out after 120s

### Model: `opencode/fledge-alpha-free`
# My Position: SDD *contains* TDD and absorbs prototypes; treating any of the three as a dogma fails

## 1. Recommendation

Use **spec-driven development with a TDD discipline inside it** as the default for anything that must live past one session, and treat prototype-based development as a discovery tool that feeds the spec — never as a substitute for it. The three aren't peers: TDD's "red-green-refactor" is a subset of what a good SDD loop already does (Spec Kit literally enforces "tests MUST be written before implementation" in its constitution), and prototyping answers a different question ("what should we build?") than SDD ("how do we build it reliably?"). So the real question is sequencing, not selection: prototype → spec → TDD-guarded implementation.

## 2. Reasoning and evidence

**Why pure prototype/vibe-first fails for anything durable:**
- A 2026 Medium experiment (Fabián Silva Ortiz, "I Built the Same API Four Times") built the same API via vibecoding, OpenSpec, Spec Kit, and Kiro. Vibecoding produced **zero tests** and opaque decisions; Spec Kit produced 38 tests plus an OpenAPI contract, correlation IDs, Dockerfile, and migrations; Kiro produced 60 tests including property-based ones. Same input prompt, wildly different artifacts.
- METR's 2025 RCT: experienced developers on their own codebases were **19% slower** with AI tools while perceiving 20% speedup. The bottleneck is precision of intent, not typing speed.
- GitClear's 211M-line study: AI adoption correlates with rising churn and ~4x code duplication. SDD's spec artifact is the memory that counters this.
- The failure mechanism is structural: an agent defect is a **gap in the spec**, and every re-prompt re-rolls that gap in a new form. Writing the constraint into a spec patches the process, not just one output.

**Why I don't pick plain TDD-with-research as the top-level frame:** TDD is excellent but it's about *verification of code*, not about *deciding what the code should be* or keeping shared intent across sessions, agents, and team members. Research/discuss/plan reviews are valuable but if the plan isn't versioned as an executable artifact, it decays the moment implementation starts. SDD's spec is the durable home for exactly that plan.

**Why prototypes still matter:** throwaway prototypes answer "what problem are we even solving?" — and forcing a spec before that learning wastes the front-loaded effort (the arXiv SDD guide explicitly warns against premature specification in exploration). ~25% of YC Winter 2025 startups are 95% LLM-generated; the failure is treating that as the finish line instead of the sketch.

## 3. Tradeoffs

- **Cost:** SDD+TDD adds real upfront time — the Medium experiment measured Spec Kit taking ~5x longer than a vibe prompt to first working code. You pay that before any output appears.
- **Risk:** spec drift. If updating the spec is slower than editing code, spec and code diverge and you've added a second source of truth to maintain. Over-specifying simple CRUD also adds pure cost.
- **Wrong for:** one-off scripts, spikes, UI exploration, solo throwaway experiments, and anything where being wrong costs nothing. There, prototype freely and accept the mess.
- **What I give up:** vibe-coding's raw speed-to-first-output (score of 5 vs SDD's 3 on that one dimension in the Felipe Fontoura comparison, for what that's worth).

## 4. What would change my mind

- If empirical results flip on METR-style studies — i.e., frontier agents reliably produce production-quality code from conversational prompts with no durable spec, making the spec redundant overhead.
- If tooling made specs automatically regenerate from code (killing drift), my case for spec *files* weakens, though the argument for explicit intent remains.
- If the work is genuinely prototype-only (throwaway, single session, no correctness bar), I'd already say skip the spec — and weight all three toward prototyping.

### Model: `opencode/ling-3.0-flash-fin-free`
> **Error:** opencode reported this session as 'failed'. The model may have been rate limited, or the agent 'plan' may not be usable.

### Model: `opencode/ling-3.1-flash-free`
I'll ground my position in the actual workspace and current industry evidence before answering.


## 1. Recommendation

**Spec-driven development as the spine — but thin specs, gated by prototypes for anything uncertain, with tests as the executable half of the spec.** Reject "prototype instead of specs" as a default; treat "TDD with research" as a component, not a methodology. Order matters: prototype the risky 20% → write a thin spec (what/why + contracts) → implement test-first where behavior is well-defined.

## 2. Reasoning and evidence

- **Specs win in the agent era because they are the steering artifact.** I looked up Martin Fowler's analysis of the SDD tools (Oct 2025): Kiro, GitHub Spec Kit, Tessl. His core finding: SDD means "writing a spec before writing code with AI… the spec becomes the source of truth for the human and the AI." GitHub's Spec Kit (open-sourced Sept 2025) formalizes specify → plan → tasks → implement → converge. Without an explicit intent artifact, agents drift; this is exactly why this workspace has `specs/` (constitution.md, mission.md, roadmap.md, validation.md) plus `DESIGN.md` — the debate app I'm answering in was built spec-first.
- **TDD's evidence is real but narrow.** The systematic review by Bissi et al. (Inf. and Software Technology, 2016; 27 studies) found ~76% of studies showed improved internal quality and 88% improved external quality — but ~44% showed *lower* productivity. Nagappan et al. (Microsoft/IBM case studies) found 40–90% defect-density reduction at a 15–35% time cost, and several replications found no significant difference. TDD is a design/feedback technique; it cannot tell you *what* to build. "TDD with research" is spec-first in disguise: the research produces the spec, TDD executes it.
- **Prototypes are unmatched for retiring uncertainty** (UI feel, unfamiliar APIs, performance), but "prototype instead of specs" creates the throwaway-becomes-production trap: implicit decisions, no testable contract, and agents inherit drift. Fowler's caution cuts both ways — MDD historically failed at the wrong abstraction level and Spec Kit generates "very verbose" artifacts — so specs must stay thin or they become overhead.

## 3. Tradeoffs

- **Costs:** upfront latency, spec maintenance, and spec-code drift. Fowler notes even Spec Kit is arguably spec-*first* only, not spec-*anchored* — living specs are hard.
- **Risks:** stale specs are worse than none; heavyweight specs recreate MDD's failure mode.
- **Wrong for:** throwaway scripts, one-off explorations, or solo work where the human *is* the spec. It's also wrong for teams that won't update specs when behavior changes.

## 4. What would change my mind

Evidence that modern agents hold intent fidelity from code + tests alone (tests as a sufficient spec); a real project where spec artifacts stayed in sync at scale without ceremony; or data showing prototype-first consistently beats spec-first on *maintained* products rather than demos. Until then: prototype to learn, spec to commit, test to verify.

*Sources verified: martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html; github.blog (Spec Kit, Sept 2025); Bissi et al. 2016 systematic review; Nagappan et al. 2008 industrial case study; workspace `specs/` directory and `AGENTS.md`.*

### Model: `opencode/longcat-2.5-preview-free`
Let me first check the workspace for any relevant context, then form my position.

Now I have enough context. This project itself uses spec-driven development (specs/ directory with constitution, mission, architecture, roadmap, validation). Let me form my position.

---

## My Position: Prototype-First, Spec-After

### 1. Recommendation

**Prototype-based development is the strongest default for most software work today — but only if paired with a "spec-after" discipline.** Don't write specs before you code. Don't write tests before you understand the problem. Build the smallest thing that could possibly work, learn from it, then write a spec that captures what you actually learned. Then iterate.

This is not "no specs." It's "specs last, not first."

### 2. Reasoning

**Spec-first fails because you're writing fiction.** When you write a spec before you've built anything, you're describing a system you don't yet understand. You're guessing at edge cases, APIs, data shapes, and failure modes. The spec feels rigorous, but it's built on untested assumptions. I've seen this repeatedly — the spec says "the API returns X," you build to the spec, and then you discover the API actually returns Y, and now you have to rewrite both the spec and the code.

**TDD-with-research is better, but still front-loads understanding you don't have yet.** TDD works beautifully when you already know what "done" looks like. But in exploratory work — which is most modern software — you don't. Writing tests first means writing tests against an imagined interface. You'll rewrite them the moment you learn something new from the implementation. The research phase helps, but research without implementation is like reading about swimming instead of getting in the pool.

**Prototyping generates real knowledge.** When you build something — even something ugly and broken — you learn things you couldn't have learned any other way. You discover the actual API shapes, the real edge cases, the performance bottlenecks, the user confusion points. This knowledge is worth more than any amount of upfront speculation. Once you have it, you can write a spec that's grounded in reality rather than theory.

**This is especially true with AI coding assistants.** When an AI can generate a working prototype in minutes, the cost of "trying something" has collapsed. The bottleneck is no longer implementation — it's *understanding what to implement*. Prototyping is the fastest way to close that understanding gap.

**Evidence from the industry:**
- Basecamp's "Shape Up" methodology (which I'm drawing from, though I haven't re-read it recently) essentially does this: you prototype in a cycle, then commit to a bet with a written spec *after* you've validated the approach.
- The Lean Startup build-measure-learn loop is this same idea applied to products: build the smallest thing, learn, then decide what to build next.
- The "Worse is Better" argument (Richard Gabriel, 1989) makes the case that simple, working prototypes beat complete, correct specs for gaining adoption and learning.

I did not look up fresh citations for this — I'm reasoning from what I know of software engineering practice and methodology literature. If you need current empirical studies on TDD effectiveness or spec-driven development outcomes, I can search for those.

### 3. Tradeoffs

**What this costs:**
- **Short-term messiness.** The first prototype will be ugly. It will have bad abstractions, hardcoded values, and missing error handling. If you don't follow through with the spec-after step, this mess becomes permanent.
- **Less upfront alignment.** If you have a team of 10 engineers, they can't all prototype independently — they'll build conflicting things. Spec-first gives everyone a shared target before they start.
- **Harder to estimate.** Without a spec, you can't give a reliable timeline. "It'll be done when it's done" is honest but unsatisfying to stakeholders.

**What it risks:**
- **Prototype-forever.** The biggest risk is that the prototype *becomes* the product. You ship the hacky first version, it works, users depend on it, and now you're stuck maintaining it forever. The spec-after step is what prevents this — it's where you pay down the technical debt the prototype accumulated.
- **Throwaway code.** If you prototype without discipline, you might throw away 80% of the work. That's fine if the learning was worth it, but wasteful if you could have known the answer upfront.
- **Not suitable for all domains.** If you're building a medical device, a financial trading system, or an aircraft control system, prototyping without specs is reckless. The cost of being wrong is too high.

**Who it's wrong for:**
- Large teams that need coordination before implementation
- Safety-critical or regulated domains
- Projects where the requirements are already well-understood and stable
- Organizations that need to commit to timelines and budgets before starting work

### 4. What Would Change My Mind

- **If you can show me that spec-first consistently produces better outcomes for the *specific type of work I'm doing*.** If the domain is stable, the requirements are well-understood, and the cost of iteration is high, then spec-first is rational. I'd want to see evidence that the specs were accurate — that they didn't need major revision after implementation began.
- **If you can show me that TDD-with-research actually speeds up delivery in exploratory work.** I'm skeptical because I think you end up rewriting the tests, but if there's evidence that the tests serve as a useful thinking tool even when they change, I'd reconsider.
- **If the team is too large to prototype independently.** At some scale, you need shared understanding before implementation, and specs are the most efficient way to create that. I don't know exactly where that threshold is, but it's real.
- **If the cost of a wrong prototype is catastrophic.** If users could lose money, data, or safety, then you need the discipline of specs and tests before you ship anything.

---

**Bottom line:** The question isn't "specs or no specs?" It's "when do you write the spec?" My answer: write it after you've built something real, not before. The spec should describe what you learned, not what you guessed.

### Model: `opencode/mimo-v2.6-flash-free`
> **Error:** Timed out after 120s

### Model: `opencode/muse-spark-1.3-contributor-free`
1. Your recommendation, stated plainly and up front.

**Default to prototype-based development, not spec files or plan files.** Build the smallest runnable thing first, then harden what survives. Write specs only to lock in behavior that has already proven useful.

For agentic / LLM-assisted work, code is cheaper than prose. A prototype is a testable claim; a spec is an untestable wish.

2. The reasoning that supports it, with evidence.

- **Specs rot faster than code in this repo context.** I did not look anything up externally for this — this is judgment from how fast LLM-generated code churns. Spec-first assumes requirements are stable and known. They rarely are until you see the thing run.
- **Prototypes resolve ambiguity that discussion cannot.** "Discuss & plan" loops tend to produce confident-sounding plans for the wrong problem. One working prototype surfaces real constraints: API shape, latency, UX awkwardness, edge cases.
- **TDD + research is strong but premature as a starting mode.** It is excellent for well-understood logic (parsers, billing, authz). It is slow for exploration, and tests written before understanding encode guesses. I would use it in phase two: once the prototype shows what matters, pin it with tests.
- **Plan files have a coordination cost.** They help large teams and long-lived systems. For small council-style parallel work, they become synchronization overhead — everyone debates the doc instead of reacting to the demo.

Practical order I defend: prototype (hours) → demo → keep/kill → tests + minimal spec for the keeper.

3. The tradeoffs: what this costs, what it risks, who it is wrong for.

- **Costs:** throwaway code; some duplication; less upfront traceability; harder to parallelize large teams because there is no contract to build against.
- **Risks:** prototype becomes production without hardening ("demo-ware"); missing security, migration, operability concerns; churn if no one enforces the kill step.
- **Wrong for:** regulated / safety-critical work, multi-team contracts, public APIs, data migrations, anything where failure cost >> iteration cost. There, spec-driven or TDD-first is correct. Also wrong for teams that cannot throw code away — if every prototype ships, you get tech debt fast.

Mitigation: timebox prototypes, define "done means tested + documented," and forbid merging prototypes without tests.

4. What would change your mind.

- Evidence that in this specific project most rework comes from missing contracts rather than wrong direction — e.g., repeated integration breakage across council members.
- A team larger than ~5 active contributors where parallel work without a spec causes constant collisions.
- Long-lived APIs or persisted data models where backward compatibility matters more than speed of discovery.

If any of those hold, I would switch to: short RFC/spec for interfaces only, prototype for everything else.

### Model: `opencode/nemotron-3-ultra-free`
## My Position: Spec-Anchored SDD with TDD at the Unit Level, Prototypes Only for Spikes

---

### 1. Recommendation

**Adopt spec-anchored Spec-Driven Development as the primary methodology, with TDD as the mandatory unit-level practice, and reserve prototype-based development strictly for time-boxed technical spikes.**

This is not a "pick one" choice—these operate at different levels of abstraction. SDD governs *what* we build and *why* (feature/system level). TDD governs *how* we build each unit (class/function level). Prototypes are a *tactic* for reducing uncertainty, not a methodology for delivery.

---

### 2. Reasoning with Evidence

**SDD is the only methodology designed for AI-assisted development.** The 2026 arXiv paper (2602.00180) and Thoughtworks analysis both position SDD as the response to AI coding agents: "Context is delivered, not discovered" (SpecD). Without a spec layer, agents hallucinate requirements, diverge from intent, and produce unmaintainable code. The three rigor levels (spec-first → spec-anchored → spec-as-source) let teams adopt incrementally. Spec-anchored—where specs evolve alongside code, bridged by automated tests—is the practical sweet spot for most teams (IBM, 2026).

**TDD is SDD at the unit level.** The same arXiv paper states explicitly: "TDD is SDD at the unit level. Writing a test first is writing a micro-specification." Empirical evidence: TDD pairs produced 18% higher functional quality but took 16% more time (ACM SAC 2003, n=24 professionals). The tradeoff is real and measured. TDD's Red-Green-Refactor cycle forces interface-first thinking and prevents the "messy aggregation of code fragments" Fowler warns about.

**Prototype-based development has a narrow, validated role.** Research distinguishes *throwaway* (discarded) from *evolutionary* (becomes final system) prototyping. The ACM mapping study (2023, 33 primary studies, 12 case companies) identifies five aspects: purpose, scope, media, use, exploration strategy. The dangers are documented: "insufficient analysis," "user confusion of prototype and finished system," "integration between iterations can be an issue" (Medium, 2020). The build-and-fix model—no specs, no design, neglected testing—is the antipattern prototype-only devolves into.

**The GSD (Graphify/Spec-Driven) tooling in this repo proves the point.** The `.planning/` directory with `SPEC.md`, `PLAN.md`, `TASKS.md`, and graph-based context compilation (`.planning/graphs/`) is spec-anchored SDD implemented. It exists because pure vibe-coding and prototype-only approaches failed to scale.

---

### 3. Tradeoffs

| Dimension | Spec-Anchored SDD + TDD | Prototype-Only |
|-----------|-------------------------|----------------|
| **Upfront cost** | Higher (specs, plans, test lists) | Lower (start coding immediately) |
| **AI agent effectiveness** | High—structured context compilation | Low—agents guess intent |
| **Traceability** | Requirements → specs → tests → evidence | Weak or nonexistent |
| **Refactoring safety** | High (TDD + spec-anchored tests) | Low (no test-first discipline) |
| **Exploration speed** | Slower for genuine unknowns | Faster for technical spikes |
| **Team onboarding** | Specs serve as living documentation | Tribal knowledge, oral tradition |
| **Regulated/compliance contexts** | Audit-ready by design | Nearly impossible |

**Who this is wrong for:**
- Solo developers building throwaway side projects
- Teams exploring genuinely novel technical territory where *no one* knows the answer (use spikes, then graduate to SDD)
- Ultra-early startups where the product concept changes weekly (spec-first is waste; use spec-anchored lightly)

**What it costs:** Discipline. Spec-anchored SDD requires maintaining specs as code evolves. TDD requires the refactor step (the step most teams skip). Both feel slower initially—the 16% TDD time premium is real.

---

### 4. What Would Change My Mind

1. **Evidence that modern SOTA agents (Claude 4, o3, etc.) can reliably infer correct specifications from vague prompts** across diverse domains without a spec layer. The Fan Zeng comment on Microsoft's SDD blog ("SOTA agents can perform spec-kit steps autonomously... much cheaper, better, faster") claims this but provides no data. Show me controlled experiments.

2. **A prototype-only codebase that scales past 50k LOC with <20% bug regression rate** and onboarding time under 2 days for new developers. The build-and-fix literature suggests this doesn't exist.

3. **A formal methodology that integrates prototype exploration *into* SDD as a first-class "spike" phase** with mandatory spec synthesis afterward. (This actually exists in GSD's `gsd-spike` → `gsd-spec-phase` flow, which I'd adopt.)

4. **Proof that TDD's 16% time premium disappears with AI-assisted test generation** while maintaining the 18% quality gain. If AI writes both tests and implementation from a spec, the cycle compresses.

---

**Bottom line:** The three approaches are not peers. SDD is the *governance layer* for AI-assisted development. TDD is the *execution discipline* at the unit level. Prototyping is a *tactical tool* for uncertainty reduction. Using prototypes *instead of* specs is category confusion—it confuses exploration with delivery.

### Model: `opencode/nemotron-3.5-lightning-free`
> **Error:** Timed out after 120s

### Model: `opencode/space-bunny-free`
## 1. My position

**Prototype-first is the primary engine. Tests are the gate, not the ritual. Specs are written *after* the decision, as decision records — never as the input to the build.**

The three options are usually framed as substitutes. They aren't — they're instruments for different kinds of uncertainty, and the mistake is picking one as a house style. My ordering:

| Uncertainty | Instrument |
|---|---|
| "Is this the right thing / how does it feel?" | Throwaway prototype on a branch, real model, real input, observed in use |
| "Does this behavior hold now and after the next refactor?" | Executable test, written *with* the behavior, frozen as the acceptance contract |
| "Why is it shaped this way / who agreed?" | ADR, written after the decision when a real tradeoff exists |

Spec-first is only correct at the third row, and only when there's an actual trade-off to record. Test-first is only correct at the second row. Everything else is a document.

## 2. Evidence

**TDD's empirical record is much weaker than its reputation.** Pančur & Ciglarič (*Inf. Softw. Technol.* 2011), a family of controlled experiments over two years: no statistically significant difference vs *iterative test-last* on productivity (p=1.0), complexity, branch coverage, or mutation score. Tosun et al. (*EMSE* 2016, 24 industry professionals): no quality difference, TDD more productive on simple tasks and **significantly less** productive on complex brownfield tasks. Nagappan & Bhat (Microsoft/IBM case studies): 40–90% lower defect density, but 15–35% more initial time. Rafique & Mišić's 2013 meta-analysis: better quality, *lower* productivity in industry. The recurring thread — their own — is that the benefit may come from short cycles, not test-before-code ordering. If that's right, you're paying TDD's ordering cost for its granularity.

**Spec-first fails at the same boundary in a different costume.** Per the research writeup I read (cultofcoders, "Nobody Reads the Spec," Aug 2026): Gabriella Gonzalez took OpenAI's Symphony `SPEC.md` — written by people with every incentive to make it work — and tried to build from it. Her conclusion is arithmetic as much as argument: the spec is roughly **one sixth** the size of the shipped Elixir implementation. Five sixths of the decisions were made by someone, not written down. Her finding that a spec precise enough to generate working code "must necessarily be contorted into code" means spec-as-source just relocates the writing into Markdown. Same report: a May 2026 "Constraint Decay" study (80 greenfield + 20 feature tasks, 8 frameworks) found capable configurations losing ~30 points of assertion pass rate from baseline to fully-specified tasks; a Feb 2026 ETH Zurich evaluation (incl. Martin Vechev) found context files "do not generally improve task success rates, while increasing inference cost by over 20%." Explicit instructions get followed; descriptive overview buys nothing. INNOQ's field report adds the mechanism: tacit knowledge can't be front-loaded, stories balloon to 1,000-line specs, and "shared documents are not shared understanding."

**Prototype-first has the best economics because code generation collapsed.** Matt Pocock's Wayfinder frame is the cleanest version: fidelity decides the instrument. Low-fidelity questions → a grilling session. High-fidelity questions → build it, because some questions *can only* be answered by a running artifact. B12's prototype-plan-stack is the concrete industrial version: 1–3 engineers, 2–3 days, 5–10k lines, then an agent slices the blob into 10+ dependent, reviewable few-hundred-line PRs behind a feature-flag gate. Note what that inverts — **the plan comes after the prototype**, and they report it's faster and less theoretical than planning first. Anthropic's "Building Effective Agents" gives the failure-surface reason: a workflow breaks in one identifiable place; an agent breaks anywhere.

**The org-level evidence points the same way.** DORA 2025 (n≈5,000): AI adoption is near-universal, throughput now improves — but **stability keeps degrading**. Their named countermeasures are not specs: small batches, frequent version control commits, aggressive rollback, strong automated testing. The 2024 special report quantified it: +25% AI adoption → −1.5% throughput, −7.2% delivery stability.

Local data point from this repo, offered as signal not proof: `specs/issues/` holds 11 shipped issues at ~65–105 lines of plan/requirements/validation each (~900 lines total, 38 files, ~95KB) against 16 backend `.py` files and 14 test files. Ratio ≈ 3:1 planning-to-code, maintained by hand, unverified by anything executable.

## 3. What this costs me

- **Stakeholder reach.** Prototypes align engineers. They don't align the person who signs off, and they produce no audit trail. Prose still has a job.
- **Provable completeness.** A prototype proves *one* case end to end. It cannot enumerate the twenty paths you didn't exercise. That's the one place TDD genuinely wins — the persisted test suite. My answer is eval sets and property tests, not ceremony, but that's me paying TDD's cost in a different currency.
- **Discipline inversion.** Specs mandate capture; my flow asks for it after the fact, when nobody feels the pain. 6 months on, someone asks "why is it shaped like this" and gets shrug. I take this hit knowingly.
- **Wrong for:** regulated, contractual, safety-critical, security-boundary work, anything with an external auditor. There, spec-as-contract is legitimately correct and I'd be arguing myself out of a job. Also wrong for teams whose failure mode is *rebuilding the wrong thing* — they need discovery and requirements work, not another prototype.
- **Brownfield tax.** Prototypes are expensive to throw away in a legacy codebase, and Tosun et al. found TDD itself degrades on brownfield. The "throw it away" premise gets weakest exactly where real systems live.

## 4. What would change my mind

The honest epistemic position: **there is no controlled trial of spec-first vs prototype-first at matched cost.** The cultofcoders piece says so explicitly. Every confident claim in this debate — including mine — rests on failure modes and economic reasoning, not measured outcomes.

I'd move toward spec-first given: (a) a real randomized comparison showing equal or better defect density and throughput; (b) evidence that agents can regenerate deterministically from a spec, which would kill my strongest objection — LLM non-determinism means the same spec yields different code, which is exactly how Model-Driven Development died in the 90s, and it makes that failure mode *worse*, not better; or (c) a team whose measured bottleneck is comprehension debt across many engineers rather than decision-making speed — Addy Osmani's framing, and for *that* problem a spec is genuinely the fix, just a spec on review grounds rather than generation grounds.

I'd move further toward prototype-first if my own loop started producing artifacts nobody read and specs nobody updated. That's the tell, and it's the one I'd check first.

**My defense in one line:** the binding constraint moved from *writing code is expensive* to *verifying and absorbing code is expensive*. Specs don't close that gap, they just move the writing earlier. Only executable things — a running slice, a passing test — actually do.

## Stage 2: Peer Debate & Cross-Examination

### Round 1

## Stage 3: Blind Peer Review

- Reviewer: `opencode/fledge-alpha-free`
- Reviewer: `opencode/ling-3.1-flash-free`
- Reviewer: `opencode/longcat-2.5-preview-free`
- Reviewer: `opencode/muse-spark-1.3-contributor-free`
- Reviewer: `opencode/nemotron-3-ultra-free`
- Reviewer: `opencode/space-bunny-free`

## Session Telemetry & Metadata

- **Participating Members:** `opencode/big-pickle`, `opencode/fledge-alpha-free`, `opencode/ling-3.0-flash-fin-free`, `opencode/ling-3.1-flash-free`, `opencode/longcat-2.5-preview-free`, `opencode/mimo-v2.6-flash-free`, `opencode/muse-spark-1.3-contributor-free`, `opencode/nemotron-3-ultra-free`, `opencode/nemotron-3.5-lightning-free`, `opencode/space-bunny-free`
- **Designated Chairman:** `opencode/space-bunny-free`
- **Caveman Mode:** `{'stages': {'positions': {'label': 'Positions', 'mode': 'full', 'tokens': {'input': 212469, 'output': 7595, 'reasoning': 7738, 'total': 227802}}, 'debate': {'label': 'Debate', 'mode': 'full', 'tokens': {'input': 214325, 'output': 3362, 'reasoning': 8268, 'total': 225955}}, 'review': {'label': 'Blind review', 'mode': 'full', 'tokens': {'input': 140108, 'output': 1375, 'reasoning': 3903, 'total': 145386}}, 'verdict': {'label': 'Verdict', 'mode': 'full', 'tokens': {'input': 9060, 'output': 2429, 'reasoning': 0, 'total': 11489}}}, 'total': 610632, 'installed': True}`

---
