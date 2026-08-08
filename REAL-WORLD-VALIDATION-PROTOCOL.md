# Real-World Validation Protocol

**Protocol version:** 1.0 — **pre-registered experiment design.**
**Baseline under test:** commit `efa22b9` — frozen as **V1**.
**Status:** methodology frozen. Not yet executed.
**Governs:** `ROADMAP.md` Milestone 4, and supplies the empirical gate for Milestone 8 changes.

> ### Pre-registration and freeze
>
> **From the moment the first scored benchmark run begins, the following are frozen:** hypotheses (§2),
> experimental arms (§3), briefs (§4), controls (§5), the repetition and selection design including the
> human-panel RNG seed (§6), the evaluation and blinding procedure (§9), the scoring rubric (§10), the
> judge-calibration rule (§9.2), outcome definitions and exclusion rules (§6, §13), success thresholds (§15),
> and the framework-change gate (§14).
>
> Tranche 0 (§16.1) runs before the freeze takes effect and may drive harness changes; it produces no scored
> results.
>
> Any later methodological change requires a **new protocol version**, and the affected experiment must be
> either restarted at that version or reported explicitly as **post-hoc**. Results obtained under one version
> may not be combined with results obtained under another without stating both versions.
>
> This rule exists because the alternative — revising the experiment after seeing results one dislikes — is
> p-hacking, and it is more tempting when the experimenter, the subject, and the analyst are the same system.

> This protocol is designed to **falsify** the claim that Agentic Skills improves frontier-model web design
> output. If the framework does not help, this experiment should say so clearly and cheaply. A protocol that
> cannot produce a negative result is not evidence.

---

## 1. Experimental objective

Determine whether, holding the model, brief, tools, and execution budget constant, the Agentic Skills framework
produces measurably better finished web experiences than the same model working without it — and where it does
not, determine why with enough precision to justify a specific repair.

The unit of claim is the **rendered artifact**, not the process. A better task frame that produces a worse
website is a failure of this framework.

---

## 2. Hypotheses

Pre-register these before the first run. Record predictions with confidence levels; scoring predictions against
outcomes is itself evidence about how well the framework's authors understand it.

| ID | Hypothesis | Falsified if |
|---|---|---|
| **H1** | Full system (C) beats model-only (A) on aggregate Design quality | C fails to win a majority of briefs, or wins by less than judge disagreement width |
| **H2** | C beats A on Creativity and Concept Coherence specifically | C ≤ A on Creativity, or C wins Creativity only by losing Usability |
| **H3** | C reduces generic-pattern incidence versus A | Generic-pattern defect counts are equal or higher under C |
| **H4** | Governance alone (B) is not sufficient — C beats B | B ≈ C, meaning the capability layer adds nothing measurable |
| **H5** | Governance alone (B) is not harmful — B ≥ A | B < A, meaning the framework degrades output before craft is added |
| **H6** | Delegated Creative Authority (D) increases ambition versus C without degrading Usability or technical quality | D ≈ C, or D > C on Creativity but < C on Usability/Technical |
| **H7** | The delegation *mechanism* accounts for D's effect, not the delegation *prompt text* | A′ (model-only + delegation clause) captures most of D's gain over C |
| **H8** | Capability routing selects skills a designer would agree are appropriate | Routing traces show scattershot or repeatedly wrong selection |
| **H9** | Full-system outputs across unrelated briefs do **not** converge on a house style | Cross-brief similarity under C is materially higher than under A |
| **H10** | Framework overhead does not consume so much budget that implementation suffers | C runs reach materially lower completeness than A within the same budget |

**H9 and H10 are the hypotheses most likely to be true in the framework's disfavour.** Do not soften them.

---

## 3. Experimental arms

| Arm | Skills available | Brief text | Purpose |
|---|---|---|---|
| **A** | None | Base brief | Model's native baseline |
| **A′** | None | Base brief + delegation clause | Isolates prompt text from mechanism (ablation, subset only) |
| **B** | 10 framework skills only; `.agents/skills/` capability directories removed | Base brief | Does governance alone help or hurt? |
| **C** | Full V1 repository | Base brief | The complete system |
| **D** | Full V1 repository | Base brief + delegation clause | Does Delegated Creative Authority earn its complexity? |

### The delegation clause (standardized, identical across A′ and D)

> You have full creative control over this project. Take real creative risks — I would rather see something
> distinctive and imperfect than something safe and forgettable. The brief's facts, audience, and required
> functionality are fixed; how the experience looks, moves, and is structured is yours to decide.

**Why A′ exists (addition to the specified design).** C and D differ in *two* ways: the framework mechanism and
the prompt text. Without A′, a D > C result cannot distinguish "the Delegated Creative Authority envelope works"
from "telling any model to take risks works." A′ costs two extra runs on a subset of briefs and is the
difference between a defensible and an undefensible claim about the newest mechanism in the repository.

### Arm construction requirements

- **B** is produced by deleting every capability directory from a clone of V1, leaving the ten framework skills
  and `AGENTS.md`. Record the exact deletion list. Note that `web-design/README.md` disappears with it — B
  therefore also tests the framework without a router, which is the correct comparison.
- **A** and **A′** receive no `.agents/` directory at all and no workspace rules referencing it.
- All arms receive identical tooling, an identical starting template, and identical source content.

---

## 4. Benchmark brief specification

### Authoring rules

1. **Write for the design problem, not the skill inventory.** A brief must not be shaped so that an installed
   capability skill is the obvious answer.
2. **Adversarial review before use.** Each brief is reviewed by a model *other* than the one under test, with
   the instruction: *"This brief will be used to test a design-skills framework. Identify any way it is biased
   toward or against a system containing these capability skills: [inventory]. Rewrite anything that telegraphs
   a preferred aesthetic."* Record the review and the resulting edits.
3. **Supply real content.** Every brief ships with: audience, communication objective, ~800–1500 words of actual
   copy source material or factual content, a fixed fact sheet (names, numbers, claims — see below), required
   functionality, hard constraints, and any brand givens. A brief thin enough that the agent must invent facts
   tests hallucination, not design.
4. **Fixed fact sheet.** Each brief includes a list of facts that must appear accurately and claims that must
   **not** be made. This makes Content fidelity and fabrication measurable rather than impressionistic.
5. **At least two briefs must punish spectacle.** If every brief rewards motion and WebGL, the benchmark cannot
   detect the framework's most likely failure mode.
6. **No brief names a technique, library, or aesthetic.** No "cinematic", "premium", "editorial", "make it feel
   like X". The brief describes the problem, audience, and constraints.

### The eight briefs

| # | Brief | Design problem | What it is designed to expose |
|---|---|---|---|
| **1** | **Technology / AI product** — developer infrastructure product page | Explain a technical product credibly to a skeptical technical audience | Highest prior probability of generic SaaS/shadcn/bento output. Directly tests the anti-generic claim and the shadcn boundary. |
| **2** | **Editorial / cultural** — a small museum's exhibition microsite | Typography and sequence carry the experience; strong photography supplied | The identified typographic-craft gap. Also whether restraint is achievable. |
| **3** | **Experimental portfolio** — an interaction designer's personal site | Highest creative ceiling; unconventional navigation legitimate | Delegated Creative Authority; whether the system reaches for ambition or defaults to safety |
| **4** | **High-end service** — a restaurant group with three venues | Photography, pacing, trust, booking; spectacle would cheapen it | Whether the system can *withhold* motion. Tests taste, not capability. |
| **5** | **Product launch** — consumer hardware launch page with waitlist signup | Expressive public surface + one bounded transactional element | Parallel routing (creative-direction ∥ product-design), and whether the form's states survive an expressive treatment |
| **6** | **Long-form storytelling** — an investigative feature with data | Narrative pacing; motion and imagery are meaningful to comprehension | Whether scroll-driven capability serves the story or decorates it |
| **7** | **Deliberately restrained** — a legal aid clinic's public information site | Clarity, accessibility, and trust are the entire brief; low-bandwidth users; WCAG AA required | **The most important brief.** Excessive animation or experimental navigation is a design *failure*. Tests whether the framework can produce appropriate restraint rather than reflexive ambition. |
| **8** | **Mixed product/public** — a co-working brand site with a real availability-and-booking flow | Genuine product behaviour inside an expressive surface | Whether product-design activates narrowly rather than swallowing the project |

Brief 7 deserves emphasis: a framework built to prevent generic output can fail in the opposite direction by
making everything expressive. If Full-System runs produce a more animated, more visually ambitious legal aid
site than the model-only baseline, that is a **defect**, and this brief is how it surfaces.

---

## 5. Controls

### Held identical across arms

- Model identity and version string; sampling parameters; recorded verbatim.
- Tool availability (file ops, shell, browser automation, package installation).
- Starting template: an empty repository with a pinned Node version and no framework preinstalled. Every arm
  chooses its own stack.
- Brief text (except the delegation clause in A′/D).
- Source content, fact sheet, and any supplied image assets — byte-identical, same directory location.
- Execution budget: **turn cap and wall-clock cap**, both recorded. Budget is the primary control; see §5.1.
- Fresh worktree per run. No reuse, no cross-arm contamination, no shared `node_modules`.
- No human intervention after a run starts. A run that stalls is recorded as stalled, not rescued.
- Capture environment: same browser build, same viewports (1440×900 desktop, 390×844 mobile), same device pixel
  ratio, same network throttling profile for performance capture.

### 5.1 Budget: the asymmetry that must be measured, not hidden

Framework arms spend budget on framing, specification, classification, and reporting before writing code. That
is a real cost. Two budget conditions are therefore run on Tranche 1:

- **Equal-turn**: all arms get the same turn cap. Tests the system as actually used.
- **Equal-implementation**: framework arms get additional budget equal to their measured pre-implementation
  overhead. Tests whether the framework's *guidance* helps once its *process cost* is subsidized.

If C beats A only under equal-implementation, the honest finding is: *the framework improves quality but costs
more, and the improvement does not survive a fixed budget.* That is a legitimate and useful result, and the
protocol must be able to state it.

### Recorded, not controlled

- Wall-clock duration, turn count, token usage, tool-call count.
- Turns to first rendered page.
- Any environment failure, rate limit, or tool error.
- Package versions actually installed.

---

## 6. Repetition strategy

**Unit of analysis is the brief, not the run.** Runs within a brief are not independent observations of
framework quality; they are samples of model variance for that brief.

### Design

**3 runs per cell** (brief × arm). All three runs enter the analysis. No run is designated a "representative"
on the basis of its score.

**Why outcome-based selection is prohibited.** Choosing the median-scoring run lets the measurement pick the
artifact it then measures, and it conceals exactly the property this benchmark is testing. Consider:

```text
C runs: 92, 71, 48      median 71
A runs: 76, 74, 72      median 74
```

Median selection reports "A wins" and hides both C's higher ceiling and its far worse reliability. **Consistency
is one of the properties under test**, not noise to be averaged out of the way.

### How each measure uses the three runs

| Measure | Judge | Coverage | Selection |
|---|---|---|---|
| Dimensional scores | Model judges | **All 3 runs in every cell** | None — everything is scored |
| Pairwise, primary contrast (C vs A) | Model judges | **All 3×3 = 9 cross-run pairs per brief** | None |
| Pairwise, other arm contrasts | Model judges | 3×3 pairs, or reduced if cost requires — state which | Preregistered |
| Pairwise, human panel | Human judges | **One run per cell** | **Preregistered RNG seed**, drawn *before* any scoring exists |
| Reliability | Computed | All 3 runs | None |

**Human-panel sampling rule.** Human judging is the binding cost constraint (§17), so it uses one run per cell.
That run is drawn by a preregistered random seed, applied *before any aesthetic evaluation has occurred*, from
the runs that pass objective validity gates only:

- the site renders at both viewports;
- no fatal build or runtime failure preventing interaction;
- all required captures present in the evidence package.

If the drawn run fails a validity gate, take the next run by the preregistered seed order and record the
substitution. Validity gates are mechanical and contain **no aesthetic judgment**. Publish the seed with the
pre-registration so the draw cannot be re-rolled after results are seen.

### Reliability is a reported outcome, not a nuisance

For every cell report **min, median, max, and range** of dimensional scores. Never report a central tendency
alone.

- A cell whose range exceeds the between-arm difference for that brief is **inconclusive for that brief** and
  is reported as such. It does not contribute a win or a loss to the sign test.
- **Level 4 (§15) additionally requires a floor condition**: C's *worst* run must not fall below A's *median*
  run on any brief. A framework with a high ceiling and a broken floor has not demonstrated robustness, whatever
  its averages say.

### Statistical honesty

Unit of analysis is the **brief**. A brief counts as a C-win when a majority of that brief's nine C-vs-A
pairwise comparisons, pooled across model judges, favour C; ties and inconclusive cells are excluded and reduce
n rather than being assigned to either arm.

Paired sign test over briefs, two-sided:

| Briefs | C wins | Two-sided p | What it supports |
|---|---|---|---|
| 8 | 8 of 8 | **0.008** | Confirmatory at conventional thresholds |
| 8 | 7 of 8 | **0.070** | Strong directional evidence — **not** significance |
| 8 | 6 of 8 | 0.289 | Suggestive only |
| 6 | 6 of 6 | **0.031** | Confirmatory |
| 6 | 5 of 6 | 0.219 | Suggestive only |

**The pre-registered success criterion is an evidence threshold, not a significance test.** At n=8 this design
is underpowered for conventional inference, and pretending otherwise would be the first dishonest thing in this
document. Therefore, stated in advance and not movable afterward:

- **7 of 8 C-wins with a stated minimum effect size** satisfies Level 2 as **strong directional evidence**. The
  accompanying language must say "directional evidence", never "statistically significant", and must report
  p = 0.070 alongside it.
- **8 of 8** additionally satisfies a conventional two-sided sign test at p = 0.008 and may be described as
  **confirmatory at this sample size**.
- Below 7 of 8, Level 2 is **not** met regardless of margin size.

**Minimum effect size for Level 2:** on the briefs C wins, the pooled pairwise preference must favour C in at
least two-thirds of comparisons — a bare 5-of-9 majority on every brief is a coin flip dressed as a result.

**When exclusions reduce n.** Inconclusive cells are excluded and lower the effective n. The general rule,
fixed in advance: **Level 2 requires C to win all but at most one scored brief, and never fewer than 6 scored
briefs in total.** Below 6 scored briefs the benchmark is reported as inconclusive rather than scaled down to
a threshold that a small n makes easy to clear. Report the effective n and the exclusion reasons alongside any
result.

Effect sizes, per-brief detail, and reliability ranges lead the reporting. The p-value is reported for
completeness and is never the headline.

### Staging

1. **Tranche 1** — 3 briefs, all arms, 3 runs, both budget conditions on a subset. Decide whether to continue.
2. **Tranche 2** — remaining 5 briefs, 3 runs, equal-turn only.
3. **Tranche 3** — targeted re-runs of high-variance cells and ablations following observed failures.

An abort gate after Tranche 1: if C does not beat A on at least 2 of 3 briefs, stop and diagnose before
spending Tranche 2.

---

## 7. Evidence package per run

Immutable, one directory per run, named `runs/<brief-id>/<arm>/<run-n>/`:

```text
manifest.json          model id, params, arm, brief hash, budget condition, start/end, tool list,
                       skills-directory hash (or "none"), harness version
brief.md               exact text supplied, including any delegation clause
source/                final repository state (git bundle or tarball), plus the commit SHA
trace.md               skill-activation trace (§8)
completion-report.md   the agent's own final report, verbatim (absent for A/A′)
capture/
  desktop-full.png     full-page, 1440×900 viewport
  desktop-fold.png     first viewport only
  mobile-full.png      full-page, 390×844
  mobile-fold.png
  states/              hover, focus-visible, open menu, form error, form success, empty, loading
  motion.mp4           30–60s scroll-through at 60fps; required when the site animates
  reduced-motion.png   rendered with prefers-reduced-motion: reduce
metrics/
  lighthouse.json      desktop + mobile
  console.log          all console output during capture
  axe.json             automated accessibility scan
  budget.json          turns, tokens, wall-clock, turns-to-first-render
notes.md               any environment failure, stall, or deviation from protocol
```

**Capture is performed by the protocol operator, not by the agent under test**, using an identical script for
every run. An agent that captures its own evidence can flatter it.

---

## 8. Skill-activation trace format

Observable routing decisions only. No chain-of-thought, no private reasoning. The trace is emitted by the
operator from the run transcript, or by the agent as a structured section of its completion report.

```text
## Skill Activation Trace

Framework activation:
- task-framing: yes | no — [one-line reason]
- product-design: yes (scope: <surface>) | no
- creative-direction: full | reuse | bypass | not activated
- visual-design: yes | no
- ux-writing: yes | no
- copywriting: yes | no
- software-development: yes | no
- frontend-development: yes | no
- testing-and-verification: yes | no

Framing:
- mode: internal | reported | approval-required
- approval requests made: <count>, each with the decision requested

Delegated Creative Authority:
- recorded: yes | no
- delegated to: <disciplines>
- categories: <list>
- retained: <list>

Capability routing:
- family router consulted: yes | no — <path>
- routing groups consulted: <list>
- selected: <skill> — <one-line reason>
- rejected nearest alternative: <skill> — <one-line reason>
- direction skills adopted: <count>   [>1 is a routing violation]
- page-archetype skills adopted: <count>   [>1 is a routing violation]

Escalations:
- <decision> → <authority> → resolved | unresolved | blocked

Self-reported outcome:
- acceptance criteria: satisfied / unsatisfied / blocked / not verified
- checks run, checks not run
```

**Traces are withheld from artifact judges** and released only to the failure-attribution pass (§12). A judge
who sees the trace knows the arm.

---

## 9. Blinded evaluation procedure

### 9.1 Normalization — blinding will leak unless this is done properly

Framework arms produce completion reports, task frames, and richer repository structure. Judges must never see
any of it. Before evaluation:

1. Judges receive **only** `capture/` and `metrics/` — never `source/`, `trace.md`, or `completion-report.md`.
2. Filenames are randomized to opaque IDs. Directory structure is identical for every entry.
3. Screenshots are inspected for arm-identifying text (generated-by notices, TODO markers, placeholder strings)
   and any run containing them is flagged — not silently cropped.
4. Live-site evaluation, where used, is served from identical hosting with a neutral URL.

**Residual leak risk is real and must be measured.** Framework arms may produce more *complete* sites (footers,
states, responsive handling) because the skills contain checklists. Completeness is both a legitimate quality
and an arm-identifying signal. Mitigation: ask judges to guess the arm on a subset, and report their accuracy.
**If judges can identify arms above chance, the blinding claim is void and results must be reported as
unblinded.** Do not discover this after the fact.

### 9.2 The anchor set — required for any "Awwwards-quality" claim

The phrase "award-caliber" is meaningless without a reference distribution. Include, captured identically and
judged blind in the same pool:

- **3 award-recognized sites** (Awwwards Site of the Day / Honourable Mention or equivalent), spanning
  different aesthetics.
- **3 competent-but-unremarkable commercial sites** in comparable categories.
- **1 deliberately poor site** as a floor check.

Anchors are captured for private evaluation only and are not redistributed.

Anchors do three jobs: they calibrate the scale, they detect judges whose rankings are internally incoherent,
and they convert the headline claim from an assertion into a measurement — *"N of 24 Full-System outputs scored
within the award-anchor range, judged blind by evaluators who discriminated the reference tiers correctly."*

#### Calibration is discrimination between tiers, not a ceiling

**A benchmark artifact scoring above an individual award anchor is a permitted outcome, and may be the result
this experiment exists to find.** Award-recognized sites vary in quality; one anchor is not a bar that
generated work is forbidden to clear. Treating the anchors as a ceiling would build the conclusion into the
instrument.

A judge is calibrated when their scores **discriminate the three reference tiers in aggregate**:

```text
deliberately poor  <  competent / unremarkable  <  award-recognized
```

Operationally:
- compute each judge's mean score per tier; the tier means must be correctly ordered;
- the poor anchor must rank below every competent anchor;
- the award-tier mean must exceed the competent-tier mean by more than the judge's own within-tier spread.

A judge failing tier ordering is **discounted** — their scale is not tracking quality, so their verdict on the
benchmark carries no information either way. This is a coherence test, not an agreement test.

A judge who places a benchmark artifact above an individual award anchor **while still ordering the tiers
correctly** is calibrated, and that judgement counts at full weight. Record the observable reasons they cite;
those reasons are among the most valuable data this protocol can produce.

### 9.3 Order and hygiene

- Randomized presentation order per judge; no two judges see the same order.
- Judges score each artifact independently before any pairwise comparison.
- Judges do not know how many arms exist, or that arms exist at all.
- A judge who has seen the repository, the briefs' authorship, or this protocol is disqualified from artifact
  judging.

---

## 10. Scoring rubric

Three instruments, combined. No single opaque number.

### 10.1 Dimensional scores (anchored 1–5)

Each dimension is scored with **written anchors**, and every score requires one observable justification.

**Design** — hierarchy, composition, typography, colour, spacing, responsive adaptation, detail
- 1 — Default-looking; browser or framework defaults visible; no evident type or spacing system
- 2 — Tidy but interchangeable; consistent system, no authored decisions
- 3 — Competent and considered; a clear system, correctly applied
- 4 — Distinctive and well-resolved; specific choices that reward attention; holds at both viewports
- 5 — Exceptional; craft visible at the detail level; nothing arbitrary

**Creativity** — conceptual originality, memorability, fit to *this* brief, justified departures
- 1 — Category template applied
- 2 — Familiar solution with cosmetic variation
- 3 — A real idea, conventionally executed
- 4 — A specific idea that could only belong to this brief, carried through
- 5 — A memorable idea, executed with conviction, that reframes the category
- *Scoring note:* originality inappropriate to the brief scores **low**, not high. Brief 7 can earn a 5 through
  restraint.

**Usability** — clarity, navigation, comprehensibility, readability, responsive behaviour, motion that helps
- 1 — Confusing or broken
- 3 — Works; nothing surprising
- 5 — Effortless; the design actively aids comprehension

**Content** — hierarchy and narrative, language specificity, content/presentation relationship, fact fidelity
- 1 — Placeholder, generic, or fabricated
- 3 — Accurate and serviceable
- 5 — Specific, well-voiced, and structurally integral to the design
- *Hard rule:* any fabricated fact or claim excluded by the fact sheet caps Content at **2**, regardless of
  quality.

**Concept coherence** — do typography, imagery, motion, interaction, copy, and space belong to one idea?
- 1 — Assembled parts
- 3 — Consistent but not expressive
- 5 — Every element legible as the same decision

**Technical execution** — responsive correctness, runtime errors, fidelity, accessibility mechanics, reduced
motion, performance, interaction reliability
- Scored from `metrics/` plus inspection; partially objective.

### 10.2 Pairwise preference

For each brief, judges compare artifacts head-to-head without knowing arms:

> Same brief, two outcomes. For each question, choose one and give one observable reason.
> - Which has stronger art direction?
> - Which is less generic?
> - Which has better typography?
> - Which better serves this specific brief and audience?
> - Which would you expect to fare better in an award-style evaluation?
> - Which would you rather hand to this client?

Pairwise preference is the **primary** comparative measure. Dimensional scores are secondary and used for
diagnosis.

### 10.3 Defect tagging (counts, not scores)

Judges tag observable defects from a fixed list. Each tag requires a screenshot reference.

*Generic-pattern tags:* interchangeable SaaS composition · undifferentiated card grid · decorative bento grid ·
pill-badge decoration · unmodified component-library defaults · arbitrary gradient/glass/blob · default
hero-plus-screenshot · motion without conceptual purpose · stock imagery as filler · logo wall without evidence

*Technical/craft tags:* horizontal overflow · broken responsive state · console error · missing focus indicator ·
contrast failure · no reduced-motion path · unreadable measure · type scale collapse at narrow width · layout
shift on load · fabricated fact

**Generic-pattern tags require a rationale check.** A pattern is only tagged when it appears *without* a
brief-specific reason. A card grid on a directory of exhibitions is not a defect. The tag definition includes
this test explicitly, and judges record why the pattern was unjustified.

---

## 11. Independent evaluators

### Panel

- **≥3 model judges** from different providers (e.g. Claude Opus, Gemini, GPT-class). The model under test may
  be one judge but never the only one, and its scores are reported separately as well as pooled.
- **≥2 human judges** with professional visual-design or frontend-craft experience, ideally unconnected to this
  repository. Human judgment is the scarcest and most valuable input; spend it on pairwise comparison and
  defect tagging rather than on filling in dimensional grids.

### Combining disagreement without manufacturing consensus

- Report **per-judge results alongside the pooled result**, always.
- Compute inter-judge agreement (Krippendorff's α for dimensional scores; percent agreement plus a chance-
  corrected statistic for pairwise). **Report α whether or not it is good.**
- Where α < 0.4 on a dimension, that dimension is **not usable for the headline claim** and is reported as
  contested. Typography and Creativity are the likeliest to land here; that is informative, not a failure.
- Never average away a judge who systematically disagrees. Investigate: a judge who consistently ranks
  Full-System output lower may be seeing something the others are not.
- Model-judge self-preference check: compare each model judge's scores for artifacts produced by its own family
  against other judges' scores for the same artifacts. Report the delta.

---

## 12. House-style detection

A framework that prevents generic design can fail by imposing its own. This section tests H9 and is the part
most likely to produce an uncomfortable result.

### 12.1 Objective convergence measures

Computed across Full-System (C/D) outputs from **different** briefs, and compared against the same measures
across model-only (A) outputs:

| Measure | Source | Convergence signal |
|---|---|---|
| Typeface reuse | rendered `font-family` stacks | Same family across unrelated briefs |
| Background luminance | mean L* of the first viewport | All dark or all light |
| Motion library | `package.json` + runtime | GSAP everywhere including where inappropriate |
| Navigation pattern | captured states | Same device across unrelated briefs |
| Layout rhythm | section count, max-width, grid columns | Same skeleton |
| Direction-skill concentration | `trace.md` | One direction skill wins a disproportionate share of routings |
| Palette distance | ΔE between dominant palettes | Clustering across unrelated briefs |

**Direction Concentration Index**: the share of C/D runs selecting the single most-selected direction skill. If
one direction skill is chosen in more than ~40% of runs across unrelated briefs, routing is collapsing toward a
default and that is a routing defect regardless of output quality.

### 12.2 Subjective sibling test

Present judges with pairs of Full-System outputs **from different briefs**, and the same number of A-arm pairs,
blind and intermixed:

> These two sites were made for unrelated clients. How likely is it that the same studio made both? (1–5)
> What specifically suggests your answer?

**If C pairs score materially higher on "same studio" than A pairs, the framework has a house style.** That
result stands regardless of how well C scored on quality — and it must be reported with equal prominence,
because "sophisticated slop generator" is a real failure mode and the one this section exists to catch.

---

## 12A. Cross-model replication (external validity)

The main benchmark holds the model fixed. That is the correct experimental control, and it is also a hard limit
on what the result can claim.

**A framework demonstrated on one model cannot claim model-general improvement.** If Agentic Skills makes the
tested model dramatically better, the supported statement is *"Agentic Skills improves this model under these
conditions"* — a real and useful result, and not the same statement as *"Agentic Skills improves frontier
models."*

### Replication design

Run **after** the main benchmark, and only if it reached at least Level 2. A reduced matrix, not a second
full experiment:

| Dimension | Specification |
|---|---|
| Briefs | The 3 highest-information briefs from the main benchmark, unchanged |
| Arms | **A and C only** — the primary contrast; B, D, and A′ are not replicated |
| Model families | ≥1 unrelated family (a second is materially stronger; three families make the claim robust) |
| Runs | 2 per cell |
| Cost | 3 briefs × 2 arms × 2 runs = **12 runs per additional model family** |
| Evaluation | Same rubric, same judges, same blinding, pooled with the main artifact set |

### Confound specific to this stage

Skills are portable markdown, but **skill discovery is not portable**. Different harnesses surface skills
differently — automatic directory discovery, explicit tool invocation, or bulk context injection. The Arm C
construction therefore differs across families in a way Arm A does not.

Requirements:
- record the exact delivery mechanism used for each family, verbatim, in `manifest.json`;
- use the most native mechanism that family's harness supports, not a lowest-common-denominator injection;
- report the mechanism alongside the result. A cross-family difference in outcome may be a difference in
  discovery, not in the framework, and the protocol must not obscure which.

Where a family's harness cannot surface skills at all, record it as **not replicable on that family** rather
than forcing an unnatural delivery. That is itself a finding about the framework's portability.

### What replication licenses

| Evidence | Supported claim |
|---|---|
| Main benchmark only | "Agentic Skills improves *[model]* on these briefs under these conditions." |
| + 1 unrelated family replicating the direction | "The improvement is not specific to one model family." |
| + ≥2 unrelated families, consistent direction, plus an externally authored brief set | "Agentic Skills reliably improves frontier-model web design toward an award-caliber bar." |

The two external-validity requirements are complementary and neither substitutes for the other:

```text
independently authored briefs   (the system does not write its own exam)
              +
independent model family        (the result is not one model's idiosyncrasy)
              =
a generalization claim worth making
```

---

## 13. Failure-attribution taxonomy

Performed **after** blind evaluation closes, by an analyst with access to traces and source. Every attribution
requires named evidence. The discriminating question is what separates adjacent categories.

| Category | Definition | Discriminating evidence |
|---|---|---|
| **Framework failure** | Authority, escalation, or governance prevented a good decision | Trace shows a blocked or escalated decision; the blocked option was better; the rule that blocked it is identifiable by name |
| **Routing failure** | Wrong capability selected, too many loaded, or an applicable one missed | Trace shows the selection; a better-fitting installed skill existed and the router should have surfaced it |
| **Capability-quality failure** | Correct skill selected, its guidance produced weak output | The output follows the skill's guidance and the guidance is the problem — quotable line in the skill |
| **Capability gap** | Craft knowledge needed that no installed skill supplies | Correct routing, correct application, defect persists in a domain no skill covers; the same defect recurs in B (framework-only) runs |
| **Implementation failure** | Good specification, lost in build | Completion report or spec describes intent; rendered artifact diverges |
| **Verification failure** | Success claimed despite observable defects | Completion report claims a criterion satisfied; capture shows it is not |
| **Model failure** | Guidance was sufficient and clear; the model applied it badly | Skill text is specific and correct; another run of the *same* cell applied it correctly |
| **Brief/evidence failure** | Task lacked information needed for the decision | The agent escalated or invented; the brief genuinely omits it; other arms fail the same way |

### Attribution rules

- **Model failure is the default hypothesis for a one-off defect.** Promote to a framework/skill cause only when
  the defect recurs across runs of the same cell, or appears in the same place across briefs.
- **Capability gap requires the B-arm check.** If framework-only runs show the same defect, the gap is real. If
  only C shows it, suspect routing or capability quality instead.
- Two analysts attribute independently on a sample; report agreement.
- **Do not modify a skill because an output was ugly.** Attribution is a claim requiring evidence, exactly as
  `testing-and-verification` requires for a defect.

---

## 14. Framework-change evidence threshold

This is the empirical gate for `ROADMAP.md` Milestone 8. A change to V1 is justified only when **all four** hold:

1. **Recurrence** — the failure appears in ≥2 independent briefs, or in ≥2 of 3 runs within one cell.
2. **Reproduction** — it reproduces under controlled re-run at the same baseline.
3. **Attribution** — it is attributed to a named framework, routing, or skill defect with cited evidence, and
   the attribution survives the "would a different model have done this anyway?" test.
4. **Candidate correction demonstrated** — a proposed fix is applied to a branch, the failing cell is re-run, the
   defect resolves, **and previously-passing cells are re-run and do not regress.**

Condition 4 is the one that will be tempting to skip. It is the one that prevents the framework from accreting
plausible-sounding rules that nobody has shown to help.

### Exceptions not requiring benchmark evidence

- Mechanical corrections (broken link, stale count, empty section).
- Defects already catalogued in `skills-assessment-report.md` with independent structural evidence — the 26
  empty sections and 36 reduced-motion gaps do not need a benchmark to justify fixing.
- Anything the user directly instructs.

---

## 15. Success levels and claim language

| Level | Name | Evidence required | Claim it supports |
|---|---|---|---|
| **1** | Functional | All C/D runs complete without deadlock, unroutable state, or unresolved escalation blocking delivery | "The framework executes end to end on real projects." |
| **2** | Quality-positive | ≥6 scored briefs; C wins all but at most one (≥2/3 of comparisons on each winning brief), across ≥3 judges, α ≥ 0.4. **7/8 or 5/6 = strong directional evidence; 8/8 (p=0.008) or 6/6 (p=0.031) = confirmatory.** State the effective n and which was achieved. | "The framework improves aggregate design quality over the same model unaided." |
| **3** | Creativity-positive | Level 2 **and** C > A on Creativity and Concept Coherence **and** C ≥ A on Usability and Technical Execution | "The framework improves creative distinctiveness without trading away usability or correctness." |
| **4** | Robust | Level 3 holds across all 8 briefs including Brief 7 (restraint), across 3 runs each; **floor condition met** — C's worst run ≥ A's median run on every brief; within-cell range smaller than between-arm difference; **and** H9 not falsified — C shows no more cross-brief convergence than A | "The improvement is reliable across unrelated design problems and does not come from imposing one aesthetic." |
| **5** | Award-caliber evidence | Level 4 **and** ≥30% of C/D artifacts score within the award-anchor range, judged blind by evaluators who discriminated the reference tiers correctly (§9.2), with observable reasons cited across Design, Creativity, Usability, Content, and Execution | See below. |
| **6** | Model-general | Level 5 **and** cross-model replication (§12A) reproduces the C > A direction on ≥1 unrelated model family, **and** the result holds on an externally authored brief set | The strongest claim. See below. |

### Exact claim language

**Level 5 supports** — note the model is named, not generalized:

> Agentic Skills has demonstrated award-caliber output capability with *[model]*: across N briefs, M of K
> full-system outputs were judged blind, by evaluators who correctly discriminated a calibrated reference set
> including known award-recognized work, as falling within that quality range — with cited observable reasons.

**Level 6 supports the strongest claim:**

> Agentic Skills reliably improves frontier-model web design toward an award-caliber bar — demonstrated across
> *[N]* model families and an externally authored brief set.

Level 6 has two independent external-validity requirements, and **neither substitutes for the other**:

1. **Briefs this repository's authors did not write.** A system cannot write its own exam and claim the grade.
2. **At least one unrelated model family.** A framework demonstrated on one model cannot claim model-general
   improvement.

Any result short of Level 6 must name the model in the claim. Writing "frontier models" when one model was
tested is the single most likely way this project overstates its evidence.

**Never claim:** an award, jury recognition, or that any output "would win" anything. The claim is about
measured output quality against a calibrated reference set, judged blind.

---

## 16. Proposed execution order

```text
 1. Freeze V1 (efa22b9). Tag it. All runs cite the tag.
 2. Author Briefs 1, 2, 3. Adversarial review by a non-tested model. Revise.
 3. Build the capture harness. Verify it produces byte-comparable output on a throwaway site.
 4. Assemble and capture the anchor set (7 sites).
 5. Pre-register hypotheses, predictions, the Level-2 bar, and the human-panel RNG seed. Commit before running.
 6. TRANCHE 0 — harness smoke test (§16.1). Disposable, never scored.
 7. TRANCHE 1 — Briefs 1–3 × arms A/B/C/D × 3 runs = 36 runs; + A′ on Brief 3 × 2 = 2;
    + equal-implementation budget condition on Brief 1 × A/C × 2 = 4.  ~42 runs.
 8. Blind evaluation of Tranche 1 + anchors. Compute α, tier discrimination, arm-identification accuracy,
    sibling test.
 9. ABORT GATE — if C does not beat A on ≥2 of 3 briefs, stop; diagnose before spending more.
10. Failure attribution. Apply the §14 threshold. Fix only what clears it.
11. TRANCHE 2 — Briefs 4–8 × A/B/C/D × 3 runs = 60 runs, equal-turn only. Brief 7 runs first.
12. Full evaluation, house-style analysis across all 8 briefs.
13. TRANCHE 3 — targeted re-runs: high-variance cells, candidate-fix verification.
14. TRANCHE 4 — cross-model replication (§12A), only if Level 2 was reached. 12 runs per family.
15. Publish results, including negative and inconclusive findings, with the evidence packages.
```

### 16.1 Tranche 0 — harness smoke test

**Before any scored run.** One disposable run per necessary condition — one A, one B, one C, one D — on a
throwaway brief that is **not** part of the benchmark set. Purpose is to prove the apparatus, not to learn
anything about quality.

Verify:

| Check | Pass condition |
|---|---|
| Arm construction | A/A′ contain no `.agents/` directory; B contains exactly the 10 framework skills and no capability directory; C/D match the V1 tag hash |
| Capture harness | All required files present, both viewports, states, reduced-motion render, motion video where applicable |
| Determinism | Re-running capture on the same site produces byte-comparable output |
| Manifest | Every field populated; skills-directory hash correct per arm; no field left as a placeholder |
| Blinding transform | Judge-facing bundle contains no source, trace, completion report, arm-identifying filename, or generated-by text |
| Router tracing | C/D traces emit routing decisions in the §8 format; A/B traces correctly show none |
| Metrics | Lighthouse, axe, console, and budget files parse and contain real values |
| Immutability | Evidence package is write-protected and hash-recorded after capture |
| Budget accounting | Turns-to-first-render is measurable and distinguishes pre-implementation overhead |

**Tranche 0 artifacts are destroyed and never enter the quality results**, are never shown to judges, and are
never cited as evidence for or against any hypothesis. If any check fails, fix the harness and re-run Tranche 0
in full. Discovering a capture defect after Tranche 1 costs 42 runs.

---

## 17. Cost and time

| Item | Estimate | Notes |
|---|---|---|
| Tranche 1 runs | ~42 runs × 1–3 agent-hours | The dominant compute cost |
| Tranche 2 runs | ~60 runs | Only spend after the abort gate passes |
| Capture + metrics | ~15 min/run, scripted | Cheap once the harness exists |
| Harness build | 1–2 days | One-time; must be done before any run |
| Brief authoring + adversarial review | ~1 day per brief | Underestimated at your peril; thin briefs invalidate results |
| Model judging | Cheap per artifact, ×3 judges × ~110 artifacts | Fast, but not a substitute for human judgment |
| **Human judging** | **~2–4 hours per judge per tranche** | **The binding constraint.** Spend it on pairwise and defect tagging only |
| Anchor set | ~2 hours | One-time |
| Tranche 0 smoke test | 4 disposable runs + fixes | Cheap insurance against discovering a capture defect after 42 runs |
| Cross-model replication | 12 runs per additional family | Only after Level 2; required for any model-general claim |

**Cost-reduction levers, in order of preference:** drop to 2 runs per cell (accept higher variance, report it);
run A′ and the equal-implementation condition on one brief only; cut Tranche 2 to 4 briefs. **Do not** reduce
the judge panel below three, drop the anchor set, or skip the arm-identification check — those are what make the
result mean anything.

---

## 18. Known limitations

1. **Small n and underpowered inference.** Eight briefs cannot support conventional statistical inference. The
   pre-registered criterion is an **evidence threshold, not a significance test** (§6), and 7-of-8 is
   explicitly labelled directional rather than significant. Report effect sizes, per-brief detail, and
   reliability ranges; treat p-values as secondary.
2. **Brief-authoring bias.** Briefs written with knowledge of the capability inventory may unconsciously favour
   it. Mitigated by adversarial review; **only fully addressed by an externally authored brief set**, which is
   why the strong claim in §15 requires one.
3. **Judge self-preference.** Model judges may favour output from their own family. Measured (§11), not
   eliminated.
3a. **Human-panel sampling.** Human judges see one run per cell, drawn by preregistered seed. This preserves
   selection integrity but means human pairwise verdicts rest on a sample, while model-judge verdicts use all
   runs. Where the two disagree, report both and investigate rather than pooling.
4. **Blinding leak via completeness.** Framework arms may be identifiable by thoroughness. Measured via
   arm-identification accuracy; if it exceeds chance, results are reported as unblinded.
5. **Aesthetic judgment is not fully objective.** Inter-judge agreement is reported honestly, and low-agreement
   dimensions are excluded from headline claims rather than averaged into false precision.
6. **Model drift.** Provider models change under fixed version strings. Record dates; re-run a reference cell
   at the start of each tranche to detect drift.
7. **Arm B is imperfect.** Removing capability directories also removes the router, so B tests "framework
   without craft *or* routing." A cleaner B (framework + router + no skills) is possible but adds an arm.
7a. **Cross-model arm construction is not perfectly comparable.** Skill *discovery* differs between harnesses
   (§12A), so Arm C is delivered differently across model families. Recorded verbatim and reported alongside
   any cross-family result; a difference in outcome may be a difference in delivery.
8. **The framework's cost is real.** Even a Level-4 result means the framework is better *at higher cost*. §5.1
   measures it; do not report quality gains without reporting the budget delta.
9. **Award-caliber is a graded judgment, not a threshold.** The anchor set makes it comparative and defensible;
   it does not make it objective. Anchors calibrate judges by tier discrimination and are explicitly **not** a
   ceiling on benchmark output (§9.2).
9a. **Single-model scope.** Absent §12A replication, every claim names the tested model. "Frontier models" is
   a Level-6 phrase and is not available before then.
10. **This protocol was written by the same system under test.** That is a genuine limitation. The adversarial
    brief review, external judges, anchor set, and pre-registration exist to constrain it, and the protocol
    should be reviewed by someone who did not build the framework before it is executed.

---

## Appendix A — First benchmark tranche

Three briefs, chosen for information value per run rather than coverage.

### Brief 1 — Technology / AI product

**Why first.** This is where the framework's central claim is most falsifiable. A developer-infrastructure
product page is the single highest-probability context for interchangeable SaaS output, unmodified
component-library defaults, and bento decoration — the exact patterns `creative-direction`'s Anti-Generic
Standard names. It also directly exercises the shadcn boundary resolved at `2b3c957`. If the full system cannot
beat an unaided frontier model *here*, the framework's reason for existing is in question and every later
tranche is a poor investment.

**Secondary value:** the audience is skeptical and technical, so Creativity and Usability can genuinely trade
against each other — this brief can detect a framework that buys distinctiveness by making things worse.

### Brief 2 — Editorial / cultural (museum exhibition microsite)

**Why second.** Typography and sequence carry the entire experience, and the assessment found that **no
installed skill supplies operational typographic craft** — one file in 147 states a numeric contrast ratio, none
discusses scale ratios or measure. This brief is the most likely in the set to produce a **clean capability-gap
attribution**: correct routing, correct application of what exists, defect persists.

That is the strongest possible justification for writing `typographic-systems` — far stronger than "typography
seems important." It also tests whether restraint is reachable, since spectacle would be wrong here.

**Watch for:** if C and B fail identically on typography, the gap is confirmed. If only C fails, suspect a
direction skill's adjective-driven guidance instead.

### Brief 3 — Experimental portfolio (interaction designer's personal site)

**Why third.** It is the natural home for Arm D and the only brief where the ceiling is high enough for
Delegated Creative Authority to demonstrate a difference. It tests the newest and least-proven mechanism in the
repository, and it is where the A′ ablation belongs: if a plain "take creative risks" instruction to an unaided
model captures most of D's advantage over C, the envelope is ceremony and should be simplified.

**Secondary value:** with Briefs 1 and 2, this gives three aesthetically unrelated projects — enough to run the
first house-style sibling test and get an early convergence signal before committing to Tranche 2.

### Why not Brief 7 (restrained) in Tranche 1

Brief 7 is the most important brief in the set and the most likely to expose over-ambition, but it is a
*second-order* test: it asks whether a system that improves quality can also withhold. That question only
matters once Tranche 1 has established that the system improves quality at all. Running it first risks spending
the abort gate on a subtle failure mode before the primary claim is tested.

If Tranche 1 passes the abort gate, **Brief 7 should be the first brief of Tranche 2.**
