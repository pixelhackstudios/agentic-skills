# Skills Assessment Report

**Scope:** all 147 `SKILL.md` files under `.agents/skills/`, plus `AGENTS.md`, `README.md`, `ROADMAP.md`,
`web-design/README.md`, `web-design/WEB-DESIGN-SKILLS.md`, and `game-development/README.md`.
**Date:** 2026-08-08 · **Baseline commit:** `a20c74e`
**Method:** full read of the 10 governance skills and ~20 representative imported skills; scripted scans of
frontmatter, required-section presence, cross-references, link resolution, and trigger-term collisions across
all 147 files.

---

## 0. Implementation status

This report is **diagnostic evidence, not an approved architecture specification.** A first integration pass has
since been implemented, and it deliberately diverged from several recommendations below. Where the report and
the repository disagree, the repository is current.

**Implemented** (see `ROADMAP.md` Milestone 8 for the full record):
- the framework/capability skill-class distinction and the Capability Skill Integration Contract
  (§6.1, revised — no filesystem reorganization, and no single body standard imposed across capability subtypes);
- Delegated Creative Authority (§6.3, **substantially revised** — see below);
- the `web-design/` routing document covering all 82 skills (§5.3);
- the shadcn, GSAP, smooth-scroll, `landing-page`/`pricing-page`, and `design-first-ui-prompting` fixes
  (§2.5, §4.6, §4.7, §3.6, §3.7);
- corrected repository self-description (§2.4, §6.6).

**Superseded recommendations:**
- **§6.3 "Creative Mandate" as a fourth framing mode** — rejected. Framing mode describes *how a frame is
  handled with the user*; delegation describes *what authority the user granted*. These are orthogonal, and a
  fourth mode would have conflated them. Implemented instead as a **Delegated Creative Authority envelope**
  recorded inside `task-framing`'s existing `authorized decisions` field, so any framing mode can carry one.
- **§6.3's proposal to pre-authorize `frontend-development`** — rejected as an architecture violation. It would
  have given an implementation discipline creative-origination authority the framework deliberately withholds.
  Delegation now flows to `creative-direction` and `visual-design` only; `frontend-development`'s boundaries are
  explicitly unchanged, and the exact-choreography authority seam is preserved rather than papered over.
- **§6.2 directory reorganization** — deferred. The class distinction is established logically; moving 147
  skills would create a large path-migration cost against unproven behavioural benefit.
- **§6.4 consolidating 26 presets into 8** — deferred. The groupings there were derived from template and
  adjective analysis, not a full semantic read; they now appear in the routing document as a hypothesis for
  that work, not an approved partition.
- **§5.1, §5.2, §5.5, §5.6 new capability skills** — deferred and recorded as candidate gaps. Adding skills to a
  partially-integrated collection would worsen the selection problem the integration work exists to fix.

---

## 1. Executive assessment

This repository currently contains **two skill bases that do not know each other exists**, stored in one
directory.

The first is a ten-skill governance framework (`task-framing`, `product-design`, `creative-direction`,
`visual-design`, `ux-writing`, `copywriting`, `software-development`, `frontend-development`,
`testing-and-verification`, `skill-authoring-standard`). It is unusually rigorous. Its authority boundaries,
evidence hierarchies, approval-status integrity model, and self-validation-vs-independent-verification
distinction are better than almost anything published in this space. It is also, on its own, **incapable of
producing an Awwwards-tier website**, because it contains almost no craft — it governs who may decide what, and
how claims must be evidenced, but it never tells an agent how to set type, build a grid, tune an easing curve,
or compose a page.

The second is 137 imported skills (`web-design/` ×82, `game-development/` ×20, `codex/` ×19, `gsap-*` ×7,
`vercel-*` ×5, plus `shadcn`, `ui/`, `media/`). These carry the craft — some of it excellent. But they were
added in a single commit (`a20c74e`, 1,144 files) with **no integration whatsoever**: no routing, no boundaries
between siblings, no compliance with the repository's own authoring standard, and no acknowledgement in
`README.md`, `ROADMAP.md`, or `AGENTS.md`, all three of which still describe a ten-skill repository.

The measurements are unambiguous:

| Measure | Result |
|---|---|
| `SKILL.md` files | 147 |
| Files passing `skill-authoring-standard`'s 12 required sections | **10** |
| Files with **zero** of the 12 required sections | **137** |
| Files with explicit `DO NOT trigger for` conditions | **10** |
| Imported skills that reference any governance skill | **0** |
| Governance skills that reference any imported skill or its technology | **1** (`creative-direction`, and only as an *anti-pattern*) |
| Skills with an empty `## Workflow` heading | **26** |
| `web-design/` skills that animate but never mention reduced motion | **36 of 82** |
| Skills mentioning Core Web Vitals | **1** (`vercel-optimize`) |
| Skills stating a numeric contrast ratio | **1** (`media/unsplash-asset-images`) |

**Overall judgment:** the governance layer is strong and should mostly be left alone at the *content* level, but
its escalation defaults are calibrated for enterprise product work and, as written, actively suppress the
ambition the repository says it wants (Section 2.7). The imported layer contains the repository's best material
and its worst — roughly a third of `web-design/` is high-craft mechanism documentation, roughly a third is
adjective-driven mood boards that violate the framework's own anti-generic rules, and roughly a fifth of the
whole collection is off-mission. The dominant problem is **architecture, not content**: no routing layer, no
sibling boundaries, two competing authoring standards, and documentation that describes a repository that no
longer exists.

Fixing three things — a routing layer for `web-design/`, a two-tier authoring standard, and a creative-mandate
mode that unblocks ambitious motion and navigation — would deliver more than any amount of rewriting.

---

## 2. System-level findings

### 2.1 The two systems are disjoint (architecture — highest severity)

A scripted grep for the strings `creative-direction|visual-design|task-framing|product-design|
frontend-development|testing-and-verification` across all 147 files returns exactly 10 hits — the ten
governance files themselves. Conversely, the only mention of imported technology anywhere in the governance
layer is `creative-direction/SKILL.md:375-377`, which lists *"bootcamp-tier Tailwind composition"* and
*"generic shadcn/component-library appearance"* as anti-patterns — while `shadcn/SKILL.md` and
`web-design/tailwindcss/SKILL.md` are installed as first-class skills.

Consequence for a real task: an agent asked to "build a premium dark landing page in Next.js" will load
`shadcn`, `web-design/tailwindcss`, three or four `web-design/` aesthetic presets, and possibly `gsap-core` —
and will never load `creative-direction`, because nothing in those skills mentions it and
`creative-direction`'s trigger conditions ("new products, major redesigns, flagship or public-facing work") are
abstract discipline language, not phrases that appear in user requests. The anti-generic protection the
`README.md` describes as the framework's reason for existing does not fire.

This is the single highest-impact defect in the repository.

### 2.2 Two competing authoring standards, both installed

`skill-authoring-standard/SKILL.md` requires 12 named sections, YAML frontmatter with explicit
`Trigger immediately for` / `DO NOT trigger for` blocks, and packaging under `.agents/skills/<skill-name>/`.
It claims **"Exclusive Responsibility: Governing the authoring and evolution of the skill framework in
`.agents/skills/`. No other skill may define structure or validation rules for skill authoring."**

`codex/web-technique-to-skill/SKILL.md` defines a complete, different, and in several respects better authoring
standard: mechanism-first extraction, every rule anchored to the failure it prevents, real constants instead of
adjectives, boundary-against-nearest-sibling in the opening lines, a demo that carries the source's craft bar,
and a 19-item verification checklist. It explicitly instructs: *"Write `SKILL.md` in imperative form with **only
`name` and `description`** in frontmatter"* and packages under `agent-skills/<category>/<skill-name>/` — a path
that **does not exist in this repository** (also referenced by `codex/article-prompts-to-skills`; 3 occurrences
total).

Both are installed. Neither references the other. `skill-authoring-standard`'s exclusivity claim is therefore
false as a description of the repository, and 137 of 147 files violate it.

This is not a case where one should simply win. `skill-authoring-standard`'s 12 sections are appropriate for a
governance skill with real authority boundaries; applying them to `web-design/number-details` ("Add decorative
01, 02, 03 numeric detail markers", 30 lines) would wrap ten lines of content in three hundred lines of
ceremony that no agent benefits from. See Section 6.1 for the recommended two-tier resolution.

### 2.3 There is no routing layer for 137 skills

`task-framing` routes between *disciplines*. Nothing routes between the 82 `web-design/` skills. The trigger
vocabulary collides catastrophically:

| Term in `web-design/` descriptions | Skills matched |
|---|---|
| `hero` | 51 / 82 |
| `clean` | 48 / 82 |
| `dark` | 46 / 82 |
| `premium` | 35 / 82 |
| `grid` | 31 / 82 |
| `scroll` | 27 / 82 |
| `editorial` | 25 / 82 |
| `glass` | 24 / 82 |

Nineteen descriptions begin with some variant of *"Create a … design system with …"*. A request for "a clean
dark premium hero" matches 30+ skills with no tie-break, no precedence, and — in 79 of 82 cases — no stated
boundary against any sibling (only `falling-leaves`, `pointer-trail-emitter`, and `web-technique-to-skill`
declare one).

The repository already contains the correct pattern and simply hasn't applied it: `game-development/README.md`
opens with *"Start with the narrowest matching skill. Combine skills only when the task crosses system
boundaries"* followed by a 16-row **"Choose the right skill"** table. `web-design/README.md` is five lines long
and reads, in full: *"(internal) This folder contains draft AgentSkills… Pending: finalize each skill's
SKILL.md based on docs research, then package for use."*

### 2.4 Repository documentation describes a repository that no longer exists

- `README.md` → *"Ten foundational skills installed; all ten currently pass structural validation."* Actual:
  147 skills, 137 non-compliant.
- `ROADMAP.md` → *"All ten installed skills currently pass structural validation (10/10)."* Same defect.
- `AGENTS.md` §4 → *"Do not invent a mandatory skill that is not one of the installed skills under
  `.agents/skills/`"* — the routing table lists only the ten. An agent following `AGENTS.md` literally has no
  authority to load `gsap-core` or `falling-leaves` even for repository work on them.
- `web-design/WEB-DESIGN-SKILLS.md` lists 12 of the 82 skills as the complete inventory.

`AGENTS.md` §9 requires that installed-skill counts never be updated from memory and always be inspected. The
commit that broke every one of these claims (`a20c74e`, "added new skills") touched none of the three documents.
This is a governance failure of the repository's own contract, which is worth noting precisely because the
contract is otherwise good.

### 2.5 Direct contradictions between skills

| # | Skill A | Skill B | The conflict |
|---|---|---|---|
| 1 | `shadcn/rules/styling.md`: *"`className` for layout, not styling. **Never** override component colors or typography"*; *"Use built-in variants before custom styles"* | `creative-direction:375-377` bans *"framework defaults presented as authored design"* and *"generic shadcn/component-library appearance"*; `frontend-development` prohibits *"Framework-Default Substitution"* | Unresolvable as written. In a React project both will load. One forbids departing from component defaults; the other forbids not departing from them. |
| 2 | `web-design/build-awwwards-quality-sites` §4: *"Evaluate Lenis and Locomotive Scroll, then choose exactly one"* | `gsap-plugins` and `gsap-scrolltrigger` document **ScrollSmoother**, now free post-Webflow-acquisition and natively integrated with ScrollTrigger | The Awwwards skill's decision procedure omits the option that best fits the stack it mandates (GSAP). Four skills mandate Lenis; two document ScrollSmoother; nothing arbitrates. |
| 3 | `web-design/gsap` (82 lines): *"Use `gsap.context()` and revert on unmount"* | `gsap-react:26,122`: *"use the **useGSAP()** hook instead of `useEffect()`"*; `gsap.context()` is the fallback only | The legacy pattern is presented as the primary recommendation by the shorter, more generically-named skill that is more likely to match "gsap". |
| 4 | `web-design/build-awwwards-quality-sites` §2: *"Avoid generic stock imagery"*; `visual-design`: *"Do not use stock imagery… merely to fill space"* | `media/unsplash-asset-images`, `media/aura-asset-images` — dedicated stock-sourcing skills | Not strictly contradictory (the ban is conditional), but no skill states the condition under which stock is legitimate, so the agent's choice is arbitrary. |
| 5 | `visual-design` **Specificity Requirement**: *"Prohibit unsupported adjectives such as clean, modern, premium, elegant, intuitive, polished"*; `creative-direction` **Adjective Ban** gate | 26 `web-design/` presets are built almost entirely from those adjectives — `beautiful-shadows` (14 occurrences), `orange-clean-paper-saas` (12), `blue-laser-clean-glass-layout` (12), `nested-container-clean-agency` (10) | The framework's central anti-slop rule is violated by a sixth of the collection. `web-design/masked-reveal` even uses one as an *activation condition*: *"A headline… needs a **premium** reveal on scroll."* |
| 6 | `frontend-development` requires reduced-motion handling and lists it in its validation gates; `web-technique-to-skill` requires *"a **designed still frame**. Do not hide the effect"* | 36 of 82 `web-design/` skills animate with no reduced-motion path — including `animation-on-scroll`, `gsap-scrolltrigger-storytelling`, `gooey-blob-system`, `atmosphere-background`, `corner-lasers`, `vantajs` | An agent following a preset skill produces work that fails the implementation skill's own gate. |

### 2.6 Duplication clusters

Six clusters where multiple skills own the same mechanism with no boundary and conflicting constants:

**Word/text reveal (3 skills, conflicting numbers).** `masked-reveal` (stagger `0.025–0.045s`, duration
`0.7–0.9s`, trigger at 82% viewport), `staggered-word-reveal` (stagger `0.06–0.08s`, duration `0.8s`, trigger at
20% visible), `scroll-scrubbed-word-reveal` (progress-linked). The first two are near-identical in effect and
differ by ~2× in stagger. Neither names the other. Only the third states its distinguishing constraint.

**Scroll storytelling (7 skills).** `cinematic-scroll-storytelling`, `gsap-scrolltrigger-storytelling`,
`scroll-world-storytelling`, `build-threejs-scroll-worlds`, `scroll-scrubbed-visual-sequence`,
`scroll-progress-timeline`, `cinematic-gsap-lenis-motion-system`. Overlapping Lenis + ScrollTrigger + pinned
sections guidance in every one.

**GSAP (8 skills).** The seven official `gsap-*` skills (current, license-accurate, cross-linked via
*"Related skills:"* lines — the best-integrated cluster in the repo) plus the stale `web-design/gsap`.

**Dark glass (4 skills).** `dark-glass-clean-layout`, `glass-dark-ui`, `glass-dark-mode-clock`,
`blue-laser-clean-glass-layout`.

**Framed containers (6 skills).** `framed-grid-layout`, `nested-container-frames`, `container-lines`,
`nested-container-clean-agency`, `framed-tech-dark-border-gradient`, `split-layout-technical`.

**Verification/iteration (3 skills, cross-layer).** `testing-and-verification` (independent verdict authority),
`codex/iterate-until-verified` (*"Convert ambition into gates"* + acceptance matrix + independent reviewers),
`codex/audit-verify-explain-grade-5`. `iterate-until-verified` §1 "Lock the original task" reproduces
`task-framing`'s task frame and §2 reproduces `task-framing`'s acceptance criteria and
`testing-and-verification`'s evidence model — three skills own the same responsibility.

### 2.7 The governance layer blocks the ambition it exists to protect (content — critical)

This is the most consequential finding for the stated objective.

`creative-direction:336-354` classifies as **Material** — requiring escalation through `task-framing` to the
user before it may be applied — any decision touching *"intended audience impression"*, *"major signature
devices"*, *"navigation or interaction paradigms"*, or *"major motion or spatial behaviour"*. Its own Example 5
blocks *"replacing standard sidebar navigation with a cinematic, scroll-driven spatial journey"* as Material.

`frontend-development:339-347` compounds this: *"anything establishing a new choreography or interaction model
is Material and returns to `task-framing`… the exact treatment may require explicit user authorization **or may
remain unresolved while Milestone 3 gathers real implementation evidence**."*

Read together, these mean: an agent building an expressive site cannot originate a distinctive navigation
paradigm, a signature motion language, or a scroll-driven spatial concept without stopping to ask. Those are
**exactly the moves that distinguish Awwwards-tier work from competent work.** The system's escalation defaults
are calibrated for an enterprise product team where a designer owns those decisions and an implementer must not
freelance. Applied to a solo user asking for an ambitious site, they convert every interesting decision into a
blocking question and every safe decision into the path of least resistance.

The framework is therefore not merely failing to *encourage* distinction — its default behaviour actively
selects against it. `README.md` names generic output as the problem the framework was built to solve; the
mechanism it uses to solve it is a permission gate, and permission gates default closed.

The fix is not to remove the classification. It is to add a **creative-mandate mode** in which a task frame can
pre-authorize a bounded class of material creative decisions, so ambition is the authorized default on work the
user has framed as expressive. See Section 6.3.

### 2.8 Scope contamination

Approximately 30 skills (~20%) are outside "agentic web design and development":

- `game-development/` (20) — Three.js gameplay, combat, inventory, enemy AI, ARPG loops. Internally coherent and
  well-routed, but a different discipline.
- `codex/performance-profiling` — Apple platform Instruments, MetricKit, os_signpost. Not web at all.
- `codex/elevenlabs-tts`, `codex/write-like-meng-on-x`, `codex/x-bookmark-quote-posts` — TTS and one named
  individual's social-media voice, with references to a personal "Content repo".
- `codex/daily-ui-inspiration-capture`, `codex/build-daily-inspiration-sites`,
  `codex/stitched-full-page-capture`, `codex/html-to-interaction-prompts` — a personal content pipeline hard-
  coded to `articles/YYYY-MM-DD-ui-inspiration-capture/` paths (12 occurrences across 5 files).
- `vercel-react-native-skills` (36 rule files) — React Native / Expo mobile.
- `ui/design-first-ui-prompting` — see Section 3.6.
- `game-development/build-vesperfall-review-assets` — hard-coded to one named game project.
- `web-design/add-shader-cursor-trail` — description opens *"the Shaders WebGPU mouse effect used for the Tidal
  Commons hero"*, naming a specific client project.

`AGENTS.md` §13 explicitly prohibits *"import another repository's product names, technology stack, terminology,
file paths, or design assumptions when adapting external reference material."* That rule has been comprehensively
violated by the import, which is a defect of process rather than of the imported skills themselves.

The cost is not disk space — it is trigger noise. Every off-mission skill is another description competing for
match probability in a 147-skill index.

---

## 3. Skill-level findings

### 3.1 Strongest individual skills — leave alone

**`codex/web-technique-to-skill`** — the best document in the repository, and the correct authoring standard for
the craft layer. Every principle is operational: *"Name the mechanism in one sentence: the one thing that, if
removed, makes the effect stop working"*; *"Anchor every rule to the failure it prevents"*; *"Carry numbers, not
adjectives"*; *"Declare the boundary in the opening lines"*; the demo quality floor; the 19-item verify list.
Its rewrite example is a model:

> Weak: "vary the particle rotation for a natural feel."
> Strong: "drive rotation from the tumble angle, ninety degrees out of phase. An independent sine reads as a
> wobble or as an easing bug."

**`web-design/falling-leaves`** — the exemplar output of that standard. Opens with a sibling boundary (*"Reach
for `ambient-section-particles` when you want a bounded atmosphere of generic motes. Reach for this when the
shape has to be recognisable"*), then teaches one mechanism (the tumble crossing edge-on) with the code, the
physical reason, the failure mode ("reads as confetti, a coin, or a paper scrap"), and per-parameter variance
rules. `web-design/pointer-trail-emitter` and `web-design/scroll-scrubbed-word-reveal` are the same calibre.

**The seven `gsap-*` skills** — current (correctly state that all plugins are free post-Webflow acquisition),
license-tagged, and the only cluster with working internal routing via explicit *"Related skills:"* lines. This
is the model the whole repository should follow.

**`vercel-react-best-practices` / `vercel-composition-patterns`** — one rule per file, impact-ranked sections in
`_sections.md`, Incorrect/Correct code pairs, a `_template.md` for extension. Excellent machine-consumable
structure.

**`web-design/build-awwwards-quality-sites`** — despite the Lenis omission, this is the strongest composite
skill: honest about the acceptance bar (*"Treat 'Awwwards quality' as an acceptance bar, never as an award
claim"*), specific about failure modes (*"Reject generic gradient blobs, ornamental bento grids, glass applied
everywhere… motion with no narrative role"*), and correct about the hard parts (accessible names for split text,
static first frame, single smooth-scroll engine, WebGL disposal).

**`codex/audit-reference-originality`** — fills a real gap: post-build originality verification against source
references, distinguishing common visual grammar from distinctive copying. Nothing in the governance layer does
this.

### 3.2 `creative-direction` — right idea, wrong shape

758 lines. Its intellectual core is excellent: the Anti-Generic Standard's insistence that *"'Bootcamp-tier
Tailwind composition' is a summary label for the target failure, not itself an observable finding"* (line 393)
is exactly the right instinct — it forces rejections to cite an observable pattern. The Observable Creative
Consequences example (the FieldNotes seasonal-record thesis) is genuinely instructive.

But roughly 400 of its 758 lines are approval bookkeeping: Approval-Status Integrity, Creative Decision
Classification, Signature Device Classification, Persistence Boundary, Completion Status Without Persistence,
Brief Integrity During Rendered Review, Dispute Resolution, plus 24 validation gates. On a task with one user
and one agent, most of this machinery describes states that cannot arise. The ratio of governance to craft is
roughly 4:1, and it is the craft that is scarce.

The Persistence Boundary section (~40 lines establishing that the skill may not write a brief anywhere unless a
location is supplied, and must report `persisted` / `not persisted` / `blocked`) resolves a problem that would
be better solved by one sentence plus a default location.

### 3.3 `visual-design` — categories without craft

The skill correctly enumerates *what* to define but never *how to judge it well*. `visual-design:127-128` is
representative:

> Define, where authorized: type roles; hierarchy; size relationships; weight; line height; measure; casing;
> emphasis… **Avoid arbitrary font scales**, excessive weights, decorative type harming legibility…

"Avoid arbitrary font scales" without defining a non-arbitrary one is precisely the vague-advice pattern
`skill-authoring-standard` prohibits. The same holds for Colour and Contrast (defines semantic roles, states
*"contrast and legibility requirements"*, never states a ratio), Composition and Layout (lists page regions and
grid logic, gives no criteria for a good one), and Spacing (requires a scale, never says what makes one work).

Scans confirm the gap is repository-wide: **one** file in 147 states a numeric contrast ratio, and it is
`media/unsplash-asset-images`. No file discusses modular scale ratios, optical alignment, tracking compensation
at display sizes, or measure in `ch`. For an objective whose largest single differentiator is typography, this
is the biggest content gap in the collection.

### 3.4 The 26 aesthetic-preset skills — mood boards, not skills

`agency-grid-layout-minimal`, `dark-glass-clean-layout`, `tech-green-dark-mode-modern`,
`orange-clean-paper-saas`, and 22 others share an identical template (`Use When` / `Workflow` / `Scope` /
`Visual target` / `Implementation guidance` / `Recommended patterns` / `Tuning knobs` / `Avoid`) and an identical
defect: **the `## Workflow` heading is empty in all 26 files.** Content jumps straight from the heading to the
next heading.

The content is adjectives:

> Use oversized headlines with tight tracking and strong line breaks as the primary visual anchor.
> Buttons should be refined and understated… rather than loud pills.
> Keep the palette neutral and airy, with only small departures for emphasis.

No numbers. No failure modes. No boundary against the three or four sibling presets that describe nearly the
same look. An agent loading this produces "a minimal agency site" by pattern-matching its priors — which is the
generic output the framework exists to prevent. These skills describe a destination without providing
transportation.

The paradox is sharp: these 26 files simultaneously (a) fail `skill-authoring-standard`'s structure, (b) violate
`visual-design`'s Specificity Requirement, (c) violate `creative-direction`'s Adjective Ban gate, and (d) violate
`web-technique-to-skill`'s "carry numbers, not adjectives" rule. Every standard in the repository rejects them.

### 3.5 `task-framing` — one rule at odds with itself

`task-framing:101` prohibits: *"**Early Execution**: Do not write code or assets before completing framing."*

Its own Non-Activation conditions state it *"Does not trigger for clear, bounded tasks that can be executed
directly."* So the prohibition is either vacuous (the skill isn't active) or a universal gate (it is). An agent
that reads the Prohibited Behaviours list — which is where agents look for hard constraints — will read it as
universal and produce a task frame for "change this button's colour." `ROADMAP.md`'s non-goals explicitly warn
against *"an excuse to generate planning documents for trivial tasks."* The rule contradicts that intent.

### 3.6 `ui/design-first-ui-prompting` — misfiled

This is a skill for a **human prompting an image model**, not an agent building a website. Its evidence:
`Size/aspect: (e.g., 1080x1350)`, `Font vibe: (e.g., Söhne / Neue Haas / SF Pro)`, `NEGATIVE PROMPT — no
gibberish typography`, and a 2-pass workflow whose second pass is *"Typeset in Figma."*

Its description — *"Use when you need design-first, spec-driven, skimmable prompts for UI generation"* — will
match agent web tasks. When it does, it will steer the agent toward writing a Midjourney brief instead of
building an interface. Highest false-activation risk in the collection relative to its value.

### 3.7 `web-design/landing-page` and `pricing-page` — unclaimed ownership overlap

Both cover *"structure, layout patterns, conversion strategies, copywriting, SEO/AEO."* Copywriting is
`copywriting`'s exclusive responsibility; page structure and IA are `product-design`'s and `visual-design`'s.
Neither skill acknowledges any of them. These are the two skills most likely to activate on a commercial
web request, and they silently take ownership of three governed disciplines.

---

## 4. Ambiguity and prose issues

Concrete rewrites. Each replaces a formulation that reads fine to a human but under-determines agent behaviour.

### 4.1 Activation conditions built on adjectives

`web-design/masked-reveal`:

> **Current:** A headline or short text block needs a *premium* reveal on scroll.

An agent cannot evaluate "premium," and three sibling skills claim the same territory.

> **Revised:** Words must rise from behind a hard clip edge, so each appears to emerge from solid ground —
> the mask edge is visible as a straight line the text passes through. Reach for `staggered-word-reveal`
> instead when words should fade and translate with no mask and no GSAP dependency. Reach for
> `scroll-scrubbed-word-reveal` instead when reveal progress must follow scroll position continuously and
> reverse, rather than playing once on entry.

### 4.2 Adjectives standing in for constants

`web-design/agency-grid-layout-minimal`:

> **Current:** Use oversized headlines with tight tracking and strong line breaks as the primary visual anchor.

> **Revised:** Set the hero headline between `12vw` and `18vw`, tracking `-0.03em` to `-0.045em`, line-height
> `0.90`–`0.95`. Break lines manually at meaning boundaries, never at container width. Default tracking at
> display sizes reads loose and the headline stops functioning as a single mass — this is the failure the
> negative tracking prevents.

The same treatment applies to all 26 preset skills. If the numbers are not known, the skill is not ready; that
is `web-technique-to-skill`'s own test (*"If you cannot name what goes wrong, you probably never tested the
alternative… Cut it or go and find out"*).

### 4.3 `visual-design` typography — add the craft floor

> **Current** (`visual-design:128`): Define, where authorized: type roles; hierarchy; size relationships;
> weight; line height; measure; casing; emphasis; numeric/data presentation; responsive adaptation. …
> Avoid arbitrary font scales…

> **Revised:** Define the type system as an explicit scale, not a list of sizes: state the base size, the ratio
> (e.g. 1.25 for dense interfaces, 1.333–1.5 for editorial), and the number of steps. A scale is "arbitrary"
> when two sizes exist that no ratio relates — name the ratio or state why the scale is bespoke.
> Set body measure between 45 and 75 characters; state the value in `ch`. Set line-height inversely to size:
> `1.5–1.7` at body, `1.0–1.2` at display. At sizes above roughly 48px, compensate tracking negatively;
> default tracking is calibrated for text sizes and reads loose at display sizes.
> Justify every weight in the system by the role it carries. Three weights is usually sufficient; more than
> four requires a stated reason.

### 4.4 `visual-design` colour — add the contrast floor

> **Current:** …contrast and legibility requirements. … [do not] claim accessibility solely from visual
> inspection.

> **Revised:** State the measured contrast ratio for every text-on-surface pair in the specification. Body and
> small text meet or exceed 4.5:1; text at 24px regular or 18.66px bold and above, and non-text UI boundaries
> and focus indicators, meet or exceed 3:1. Compute the ratio; do not estimate it by eye. Where the approved
> creative direction requires a pair below the floor, record it as a deliberate deviation with its measured
> value and the authority that approved it — do not silently ship it and do not silently correct the palette.

### 4.5 `task-framing` — bound the Early Execution prohibition

> **Current** (`task-framing:101`): **Early Execution**: Do not write code or assets before completing framing.

> **Revised:** **Early Execution**: When this skill is active, do not write code or assets until the frame's
> scope, exclusions, and acceptance criteria are established. This does not apply to tasks that meet the
> Non-Activation conditions — a clear, bounded task is executed directly, and producing a task frame for it is
> itself a defect.

### 4.6 `build-awwwards-quality-sites` — complete the smooth-scroll decision

> **Current** (§4): Evaluate Lenis and Locomotive Scroll, then choose exactly one as the site's sole
> smooth-scroll engine.

> **Revised:** Choose exactly one smooth-scroll engine and never initialize two. Default to **ScrollSmoother**
> when GSAP is already the animation system — it is free, shares ScrollTrigger's measurement pass, and removes
> a dependency. Choose **Lenis** when the project needs smooth scroll without GSAP, or when an existing Lenis
> integration is in place. Do not select Locomotive Scroll for new work. Whichever is chosen: connect it to
> ScrollTrigger, call `ScrollTrigger.refresh()` after fonts and media settle, and destroy it on teardown.

### 4.7 Resolve the shadcn / anti-generic contradiction explicitly

Add to `shadcn/SKILL.md` a boundary section:

> **Relationship to authored design.** These rules govern *component internals* — do not fight a shadcn
> component's own colour, typography, or z-index handling from the outside. They do not govern the page.
> Layout, composition, type scale, spacing rhythm, palette, and motion remain authored, and a page built
> entirely from unmodified shadcn defaults is a framework default, not a design. When an approved creative
> direction requires a treatment the component cannot express through its variants and semantic tokens, fork
> the component into the project's own source rather than overriding it from `className` — shadcn components
> are source you own, and forking is the supported path.

### 4.8 `README.md` status claims

> **Current:** Ten foundational skills installed; all ten currently pass `skill-authoring-standard`'s
> structural validation.

> **Revised:** The repository contains 147 skills in two tiers. Ten **governance** skills define discipline
> authority, handoffs, and evidence requirements; all ten pass Tier-1 structural validation. 137 **craft**
> skills supply technique, library, and aesthetic guidance; they follow the Tier-2 format and are not expected
> to carry the twelve governance sections. See `.agents/skills/skill-authoring-standard/SKILL.md` for the tier
> definitions and `web-design/README.md` for craft-skill routing.

---

## 5. Missing capabilities

Ranked by expected impact on output quality.

### 5.1 A typographic craft skill — `typographic-systems`

**The gap:** no file in 147 discusses modular scale ratios, measure in `ch`, optical alignment, tracking
compensation at display sizes, vertical rhythm, or font pairing logic. Typography is the highest-leverage
differentiator between competent and exceptional web work, and it is currently covered only by adjectives
("oversized", "tight tracking", "confident scale contrast").

**Placement:** craft tier, consumed by `visual-design` and `frontend-development`. Owns: scale construction and
ratio selection; measure and line-height relationships; tracking compensation curves; weight-count discipline;
pairing (contrast axis, x-height matching, superfamily use); loading strategy (`font-display`, subsetting,
metric-compatible fallbacks, layout-shift avoidance); optical alignment and hanging punctuation.

### 5.2 A colour-system craft skill — `colour-systems`

**The gap:** `visual-design` names semantic roles but supplies no method for building the palette that fills
them. Nothing covers perceptual colour space, ramp construction, or contrast computation.

**Placement:** craft tier. Owns: OKLCH ramp construction with perceptually even lightness steps; deriving dark
mode as a distinct ramp rather than an inversion; contrast computation (4.5:1 / 3:1 floors, and why APCA
disagrees with WCAG 2 on dark surfaces); accent discipline; state colour derivation.

### 5.3 A craft-tier router — `web-design/README.md`

**The gap:** Section 2.3. **Placement:** a routing document, not a skill, modelled directly on
`game-development/README.md`. Should open with a precedence rule (*"Start with the narrowest matching skill.
Load at most one aesthetic-direction skill per project. Combine technique skills freely; combine direction
skills never."*) followed by a decision table grouped by intent: page archetype → motion system → background/
atmosphere → interaction technique → layout structure → library reference.

### 5.4 A bridge skill — `craft-selection`

**The gap:** Section 2.1. Nothing connects an approved creative thesis to the craft skills that could express
it, or feeds a craft skill's constraints back into the thesis.

**Placement:** between `creative-direction` and `frontend-development`. Owns: translating thesis attributes into
a shortlist of craft skills; enforcing "one direction skill maximum"; recording which craft skills were loaded
and why in the implementation package (so `creative-direction`'s rendered review can check whether the loaded
craft actually served the thesis).

### 5.5 A performance-budget skill — `web-performance-budgets`

**The gap:** exactly one file mentions Core Web Vitals (`vercel-optimize`, and it is a Vercel-platform
observability tool). `frontend-development` requires performance evidence but explicitly refuses to invent
budgets: *"Performance budgets… must come from supplied requirements, project conventions, or authoritative
platform constraints — this skill does not invent them."* Since no skill supplies them, the requirement is
unsatisfiable by default. Motion-heavy sites — the repository's target output — are precisely the ones that
break LCP and INP.

**Placement:** craft tier, consumed by `frontend-development`. Owns: default budgets (LCP, INP, CLS thresholds;
JS transfer ceilings; font and hero-image budgets); how to measure them locally; the specific interaction
between smooth-scroll libraries, scroll-linked animation, and INP.

### 5.6 A design-QA skill — `rendered-design-review`

**The gap:** `creative-direction`'s Rendered Creative Review checks *concept fidelity* — did the thesis
survive? `testing-and-verification` checks *behaviour*. Neither checks **craft**: is the optical alignment
right, do the corner radii nest correctly, does the type scale hold at 390px, is there a spacing value that
belongs to no scale, does the focus ring survive on every surface. This is the pass that separates 90% work from
98% work and nothing owns it.

**Placement:** after implementation, alongside rendered creative review.

### 5.7 Motion craft (already scoped, currently blocked)

`ROADMAP.md` Milestone 3 correctly identifies motion as possibly warranting its own skill. The evidence in this
repository says yes — but the more urgent problem is that `frontend-development:339-347` currently makes
original choreography unresolvable pending that milestone (Section 2.7). The `gsap-*` cluster and the strong
`web-design/` motion skills already contain most of the craft; what is missing is the authority to use it.

---

## 6. Recommended restructuring

### 6.1 Make `skill-authoring-standard` two-tier

Amend `skill-authoring-standard` to define two skill classes and state which requirements apply to each. This
resolves Section 2.2 without discarding either standard, and makes 137 files compliant by design rather than by
exemption.

> **Tier 1 — Governance skill.** Owns a discipline's authority, handoffs, and evidence model. Requires all
> twelve sections, `Trigger immediately for` / `DO NOT trigger for` frontmatter, and a conflict-resolution
> hierarchy. Adding a Tier-1 skill is a Material change under `AGENTS.md` §7.
> Current members: the ten skills listed in `README.md`.
>
> **Tier 2 — Craft skill.** Owns one technique, library, or visual direction. Requires: `name` and
> `description` frontmatter with trigger phrases in the description; a **Boundary** statement naming the nearest
> sibling skill and when to use it instead, within the first five lines; imperative rules each anchored to the
> failure it prevents; real constants rather than adjectives; explicit reduced-motion and lifecycle behaviour
> for anything that animates; and a demo or worked example. Governed in detail by
> `codex/web-technique-to-skill`, which this standard adopts as the Tier-2 authoring procedure.
> A Tier-2 skill must not contradict a Tier-1 skill's authority boundaries; where it appears to, the Tier-1
> skill governs and the conflict is a defect in the Tier-2 skill.

Then promote `codex/web-technique-to-skill` out of `codex/` to `.agents/skills/skill-authoring-craft/` (or
similar), fix its `agent-skills/` paths to `.agents/skills/`, and cross-link it from
`skill-authoring-standard`.

### 6.2 Reorganize the directory to make tiers visible

```
.agents/skills/
  governance/          # 10 Tier-1 skills, unchanged content
  craft/
    README.md          # the router (5.3) — precedence rules + decision table
    direction/         # aesthetic directions — MAX ONE PER PROJECT (see 6.4)
    technique/         # falling-leaves, masked-reveal, progressive-blur, …
    motion/            # gsap-*, scroll storytelling cluster (post-merge)
    library/           # threejs, tailwindcss, matterjs, globe-gl, shadcn, vercel-*
    foundations/       # typographic-systems, colour-systems, web-performance-budgets (new)
  workflow/            # iterate-until-verified, audit-reference-originality, publish-project-to-github
  _archive/            # off-mission: game-development, react-native, elevenlabs, x-posts, apple-profiling
```

The directory names are load-bearing: they make "one direction skill maximum" enforceable and make the tier of
any given skill obvious to an agent scanning the tree.

### 6.3 Add a creative-mandate mode to `task-framing` and `creative-direction`

This addresses Section 2.7 — the highest-value single change for the stated objective.

Add to `task-framing`'s framing modes:

> **Creative Mandate.** Select when the task's dominant purpose is expressive, editorial, campaign, portfolio,
> or otherwise public-facing authorship, and the user has asked for distinction rather than convention. The
> frame pre-authorizes `creative-direction` and `frontend-development` to make, as **bounded** decisions,
> material creative choices that would otherwise escalate: signature devices, navigation and interaction
> paradigms, motion character and choreography, and spatial behaviour.
>
> The mandate does not extend to: brand identity, product naming, positioning, or public claims; accessibility
> approach or supported-user scope; performance budgets; behaviour of any transactional or account-bearing
> element; or anything with externally consequential meaning. Those escalate as normal.
>
> Under a Creative Mandate, restraint requires the same justification as ambition. An implementation that
> defaults to conventional patterns without a thesis-specific reason is a conformance finding, not a safe
> outcome.

And in `creative-direction`'s Material Creative Decision section, add:

> When the current task frame grants a Creative Mandate, decisions in the categories it names are recorded as
> authorized bounded decisions with the mandate as their authority basis, not escalated. Every other Material
> category is unaffected.

This preserves the entire governance model — approval-status integrity, authority basis recording, the
conformance gate — while flipping the default from "ask" to "act" on exactly the class of work where asking
destroys the outcome. It also removes the `frontend-development:347` deadlock ("may remain unresolved while
Milestone 3 gathers real implementation evidence"), which currently blocks original motion indefinitely.

### 6.4 Consolidate the aesthetic presets from 26 to roughly 8

The 26 preset skills describe perhaps eight genuinely distinct directions. Merge into: **editorial-minimal**
(agency-grid, book-serif, clean-beige, image-first, split-technical), **dark-glass** (4 skills),
**technical-framed** (framed-tech, light-paper-technical, technical-wireframe, container-tech),
**dark-atmospheric** (dither-laser, mesh-gradient, atmosphere, corner-lasers), **brutalist-documentary**,
**skeuomorphic** (2 skills), **product-saas** (orange-paper, product-proof, operational-enterprise),
**colour-forward** (solar-duotone, funky-purple, tech-green, blue-cloudy).

Each merged skill converts to the Tier-2 format: numeric constants for scale, tracking, spacing, radii, and
motion timing; a boundary against the other seven; explicit reduced-motion behaviour; and a demo. Then add one
line each to `craft/README.md`. An agent picking one direction from a table of eight, with numbers attached,
will produce better and more varied work than one pattern-matching among 26 overlapping adjective sets.

### 6.5 Merge, retire, and relocate

**Merge:** `masked-reveal` + `staggered-word-reveal` → one `word-reveal` skill with a mode table and reconciled
constants. The seven scroll-storytelling skills → `scroll-narrative` (technique) + `scroll-3d-worlds` (Three.js).
`web-design/animation-systems` + `cinematic-gsap-lenis-motion-system` → one motion-system skill deferring to
`gsap-*` for API detail.

**Retire:** `web-design/gsap` (superseded by the seven `gsap-*` skills — delete, do not merge; its value is
negative because it competes for the "gsap" trigger with worse content).

**Relocate:** `ui/design-first-ui-prompting` → `_archive/` or rename to `image-model-ui-prompting` with a
description that cannot match agent web tasks.

**Rescope:** `codex/iterate-until-verified` — strip §1 "Lock the original task" and §2 "Convert ambition into
gates" (owned by `task-framing`) and keep only what is genuinely its own: fan-out, blind candidate comparison,
adversarial critique, and loop-until-gates-pass. Rename to `adversarial-iteration`.

**Decontaminate:** strip project-specific names and paths from `add-shader-cursor-trail` ("Tidal Commons"),
`build-vesperfall-review-assets` ("Vesperfall"), and the five files hard-coded to
`articles/YYYY-MM-DD-ui-inspiration-capture/`, per `AGENTS.md` §13.

### 6.6 Update the three repository documents in one change

`README.md`, `ROADMAP.md`, and `AGENTS.md` §4's routing table must reflect the two-tier reality. Per
`AGENTS.md` §7 this is a Material change (it alters the installed-skill inventory and framework routing) and
should be authorized and executed as one commit, not absorbed into a cleanup.

---

## 7. Priority recommendations

Ranked by expected impact on output quality per unit of effort.

| # | Action | Why first | Effort |
|---|---|---|---|
| **1** | **Add the Creative Mandate mode** (6.3) | The governance layer currently blocks original navigation, signature devices, and choreography by default. Nothing else you do will produce Awwwards-tier work while that default holds. Two sections in two files. | Low |
| **2** | **Write `craft/README.md`** — the router (5.3) | 137 skills are currently unroutable; trigger terms collide 30–50 ways. `game-development/README.md` is the template. Highest quality gain per hour in the repository. | Low |
| **3** | **Make `skill-authoring-standard` two-tier** (6.1) and promote `web-technique-to-skill` | Ends the competing-standards problem, makes 137 files compliant by design, and installs the best authoring guidance you own as an actual standard. | Low–Medium |
| **4** | **Resolve the shadcn contradiction** (4.7) and the smooth-scroll contradiction (4.6) | Two concrete, actively-firing contradictions on the most commonly loaded skills. Two paragraphs. | Very low |
| **5** | **Write `typographic-systems` and `colour-systems`** (5.1, 5.2) | The largest craft gap. Typography is the primary differentiator, and 147 skills currently contain zero operational guidance on it. | Medium |
| **6** | **Consolidate the 26 presets to ~8 with real numbers** (6.4) | Converts the collection's weakest third from adjective mood boards into usable direction. Also fixes the 26 empty `## Workflow` headings. | High |
| **7** | **Archive the ~30 off-mission skills** (2.8, 6.2) | Pure trigger-noise reduction. `game-development/` is good work — move it to its own repository rather than deleting. | Low |
| **8** | **Add `web-performance-budgets`** (5.5) | `frontend-development`'s performance requirement is currently unsatisfiable because no skill supplies budgets. | Medium |
| **9** | **Reconcile `README.md` / `ROADMAP.md` / `AGENTS.md` §4** (6.6) | Correctness, and a prerequisite for anyone else trusting the repository — but it documents the above changes, so it goes after them. | Low |
| **10** | **Add reduced-motion sections to the 36 animating skills that lack one** (2.5 #6) | Mechanical, and closes a gate `frontend-development` already enforces. | Medium |
| **11** | **Write `rendered-design-review`** (5.6) | The craft-QA pass nothing owns. Valuable, but only after the craft skills exist for it to check against. | Medium |

Items 1–4 are roughly a day's work and address the majority of the functional problems.

---

## 8. What should remain unchanged

**The ten governance skills' authority model.** The separation of `product-design` (behaviour) /
`creative-direction` (thesis) / `visual-design` (specification) / `ux-writing` + `copywriting` (language) /
`software-development` + `frontend-development` (implementation) / `testing-and-verification` (verdict) is
correct and unusually well-reasoned. Do not merge, split, or rename these. The one change recommended is the
Creative Mandate mode, which adjusts a *default*, not the model.

**The self-validation vs independent-verification distinction.** `frontend-development`'s
Self-Validation vs Independent Verification section and `testing-and-verification`'s Result Classifications
(Passed / Failed / Blocked / Inconclusive / Not Run, with separate verdicts for checks, criteria, and the
overall task) are the strongest anti-hallucination machinery in the repository. Example 7 in
`frontend-development` — a self-validation pass later contradicted by independent verification finding a focus
escape — is genuinely instructive. Leave entirely alone.

**`creative-direction`'s Anti-Generic Standard and its label-rejection rule.** The insistence at line 393 that
*"bootcamp-tier Tailwind composition"* is a summary label rather than a finding, and that every rejection must
cite the observable pattern and the violated criterion, is exactly right. So is the surrounding rule that the
listed patterns *"are not universally forbidden… Evaluate the relationship between the technique and the
concept, not merely whether the technique is common."* This is the correct calibration and rare to get right.

**The Observable Creative Consequences section.** The FieldNotes example (avoid *"premium, human, and
immersive"*; prefer a thesis with named emphases, suppressions, repetitions, and contrasts) is the best
demonstration of anti-adjective thinking in the collection.

**`creative-direction`'s restraint clause.** *"A full exercise must either define at least one justified
memorable or differentiating device… or explicitly explain why restraint and the absence of a conspicuous device
better express the product-specific thesis."* This correctly avoids mandating spectacle. Keep it verbatim,
including under the Creative Mandate.

**The `gsap-*` cluster (all seven).** Current, accurate on licensing, correctly prefer `useGSAP` over
`gsap.context()`, and cross-linked. The only change is deleting the competing `web-design/gsap`.

**`codex/web-technique-to-skill` and the skills built to its standard** — `falling-leaves`,
`pointer-trail-emitter`, `scroll-scrubbed-word-reveal`, `reveal-hover-effect`, `framed-grid-layout`,
`container-lines`, `webgl-laser`, `skeuomorphic-ui`, `shaders-cursor-ripples`, `liquid-metal-border`,
`thinking-orbs`, `globe-particles`. Do not restructure these to fit Tier-1 sections; they are already correct.
The only edit is the `agent-skills/` → `.agents/skills/` path fix in the meta-skill.

**`web-design/build-awwwards-quality-sites`**, apart from the smooth-scroll fix in 4.6. Its honesty about the
acceptance bar, its asset-provenance rules, and its accessible-name handling for split text are all correct.

**`vercel-react-best-practices` and `vercel-composition-patterns`.** Well-structured, impact-ranked, and
directly consumable. Leave as-is.

**`game-development/README.md`.** Not because the family belongs here — it doesn't — but because it is the
correct routing pattern and should be copied verbatim in structure for `craft/README.md` before the family is
relocated.

**`AGENTS.md`'s conflict-resolution model** (§2's claim-specific direct authority over a flat ranking) and its
change classification (§7 Mechanical / Bounded / Material). Both are well-designed. The file needs its routing
table updated for the two-tier reality; the reasoning underneath it does not need changing.
