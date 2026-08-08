# Roadmap

Agentic Skills is being developed as a composable governance and execution framework for AI agents building web
products and digital experiences.

The framework must support both:

- interactive applications with workflows, state, permissions, and transactional behaviour;
- authored public experiences such as editorial sites, portfolios, campaigns, launch pages, and expressive
  marketing sites.

The roadmap prioritizes closing real capability gaps rather than adding governance layers for their own sake.

## Current foundation

### Completed

The foundational architecture is installed and considered stable:

- `skill-authoring-standard`
- `task-framing`
- `product-design`
- `creative-direction`
- `visual-design`
- `ux-writing`
- `copywriting`
- `software-development`
- `frontend-development`
- `testing-and-verification`

The current framework provides:

- bounded task scoping and authority control;
- conditional and parallel routing between product-led and creative-led work;
- product behaviour and workflow definition;
- expressive creative direction and anti-generic conformance review;
- visual specification;
- interface language;
- public-facing promotional and campaign copy, executing within approved creative direction;
- general software implementation;
- specialist frontend implementation, jointly activated with general software implementation;
- independent, evidence-backed verification.

The foundation should not be broadly redesigned unless real use exposes a concrete defect.

All ten installed framework skills currently pass `skill-authoring-standard`'s framework-skill structural
validation (10/10) — required sections, frontmatter, and anchors are complete and consistent. Structural
validation is distinct from real-world validation: the latter (Milestone 4) is active and not yet complete.

### Capability layer

A second class of skill is installed: 137 capability skills across `web-design/` (82), `game-development/` (20),
`codex/` (19), `gsap-*` (7), `vercel-*` (5), `shadcn`, `ui/`, and `media/`. These supply craft, technique,
library knowledge, visual direction, and reusable workflows; they execute within the framework layer's authority
rather than defining authority themselves.

They were imported in bulk and are being brought up to `skill-authoring-standard`'s Capability Skill Integration
Contract incrementally. **Installed does not mean conformant for this layer.** Known defects are catalogued in
`skills-assessment-report.md` and tracked in Milestone 8 below.

---

## Milestone 1 — Copywriting

**Status:** Completed

Created:

```text
.agents/skills/copywriting/SKILL.md
```

### Responsibility

Own promotional, persuasive, campaign, editorial, and public-facing brand-voice language, including:

- headlines and hero statements;
- page-level narrative;
- product and brand storytelling;
- campaign messaging;
- editorial introductions and transitions;
- persuasive calls to action;
- about-page and positioning-supporting copy;
- public-facing tone execution.

### Boundaries

`copywriting`:

- consumes an approved `creative-direction` thesis when one exists, or bounded direct task-frame grounding when
  no thesis exists and no material brand-voice decision is involved;
- applies supplied brand voice and terminology;
- distinguishes approved copy from newly proposed copy;
- defines observable copy acceptance criteria;
- hands frontend-rendered copy to `software-development`/`frontend-development`, with independent verification
  by `testing-and-verification` only when required — not automatically mandatory.

It does not:

- own interface utility copy, validation, errors, or system feedback — those remain with `ux-writing`;
- define product behaviour or workflows;
- establish product naming, brand positioning, legal meaning, or material public claims without authorization;
- silently create unsupported promises, guarantees, evidence, or customer claims;
- modify production code.

### Completion criteria

- Authority boundaries do not overlap with `ux-writing` or `creative-direction`.
- Public-facing and editorial copy have a clear owner.
- Creative direction can hand verbal-character requirements to copywriting.
- Examples cover campaign, editorial, persuasive, and brand-narrative work.
- The skill supports both restrained and highly expressive writing without forcing marketing language onto every
  public surface.

---

## Milestone 2 — Frontend Development

**Status:** Completed

Created:

```text
.agents/skills/frontend-development/SKILL.md
```

### Purpose

Provide specialist frontend implementation authority, jointly activated with `software-development`'s governing
baseline, for any production change to a frontend implementation surface — regardless of visual ambition.
"High fidelity" is the execution standard applied once implementing, not a threshold for whether this skill
activates.

### Responsibility

Own implementation of approved frontend requirements — supplied through applicable specifications or a bounded
task frame — including:

- semantic HTML;
- CSS architecture;
- responsive behaviour;
- component implementation;
- client-side interaction;
- framework integration;
- design-token application;
- visual-reference fidelity;
- browser behaviour;
- loading and rendered-state implementation;
- implementation-proximate accessibility;
- frontend performance;
- implementation of approved motion and interaction behaviour.

### Boundaries

`frontend-development` consumes, as applicable:

- product behaviour from `product-design`, when applicable;
- an approved thesis from `creative-direction`, when one exists;
- exact visual specifications from `visual-design`, when one exists and reference fidelity is required;
- interface strings from `ux-writing`, when applicable;
- public-facing copy from `copywriting`, when applicable;
- executable task boundaries from `task-framing`;
- otherwise, direct grounding in the task frame and established conventions when no upstream package applies.

It does not:

- redesign the product while implementing it;
- replace approved composition with framework defaults;
- simplify distinctive work into generic component-library patterns;
- invent copy, states, interactions, or brand decisions;
- claim fidelity from code inspection alone, or claim external reference fidelity when no reference target
  exists;
- substitute self-validation for independent verification when independent verification is required.

### Completion criteria

- Clearly inherits implementation discipline from `software-development` through joint activation, not a
  separate procedure.
- Covers both application interfaces and expressive public sites.
- Includes explicit protection against framework-default and component-library drift.
- Requires rendered comparison against an approved reference or specification when one exists or fidelity is
  required; states explicitly when no external fidelity claim is made.
- Defines practical frontend evidence: browser execution, viewport checks, interaction checks, visual comparison,
  accessibility inspection, and performance evidence where relevant.
- Requires independent verification by `testing-and-verification` only when the task frame, risk, or acceptance
  criteria call for it.

---

## Milestone 3 — Authored Interaction and Motion

**Status:** Active validation pending during real frontend implementation

`frontend-development` implements approved motion within its own scope as a distinct, separable section (see
its Motion Implementation section) so that a future independent motion skill could be split out without
restructuring the rest of that file. Installing `frontend-development` does not resolve whether authored motion
belongs inside it permanently or requires a separate specialist skill — that determination still requires real
implementation evidence.

Potential separate skill:

```text
.agents/skills/motion-development/SKILL.md
```

A separate skill should be created only if real implementation work demonstrates that motion has enough distinct
procedure, authority, and verification requirements to justify independent ownership.

**Delegated Creative Authority does not close this seam.** A delegation authorizes `creative-direction` to
originate motion character, concept, and meaning without escalating each decision, which resolves most cases in
practice. Where an approved motion concept still does not determine the exact authored sequence, the authority
over that exact treatment remains open and returns to `task-framing`. That residual is the evidence this
milestone is waiting on: if real work repeatedly hits it, the case for `motion-development` is made; if the
delegation route resolves it in practice, the case is not.

### Capability requirements

Whether implemented inside `frontend-development` or separately, the framework must support:

- transition choreography;
- scroll-linked and narrative motion;
- state transitions;
- entrance and exit sequencing;
- gesture and pointer response;
- spatial continuity;
- timing, easing, and orchestration;
- reduced-motion behaviour;
- interruption and reversal;
- performance-conscious animation;
- motion fidelity to the approved creative thesis, when one exists.

### Boundaries

Motion implementation must not:

- invent product behaviour;
- add animation merely to make a page feel more "premium";
- obscure content or interaction;
- override reduced-motion requirements;
- use heavy effects without performance evidence;
- replace a missing creative concept with spectacle.

---

## Milestone 4 — Real-World Validation

**Status:** Active

Use the framework on complete projects representing different work types. Motion-boundary evidence for
Milestone 3 is collected as part of these real frontend-implementation validation projects, not as a
speculative standalone design exercise.

### Validation projects

At minimum:

1. **Interactive application**
   - workflows;
   - state handling;
   - utility copy;
   - permissions or transactions;
   - responsive application UI.

2. **Expressive public website**
   - creative thesis;
   - editorial or campaign copy;
   - authored composition;
   - imagery and typography direction;
   - distinctive interaction or motion.

3. **Mixed website**
   - expressive public surface;
   - one or more bounded interactive elements;
   - parallel creative-direction and product-design routing.

### What to evaluate

- Did the correct skills activate?
- Was unnecessary product-design work avoided?
- Did creative direction materially prevent generic output?
- Did downstream implementation preserve the approved thesis?
- Were authority disputes routed correctly?
- Did any skill repeatedly lack information or overreach?
- Did the framework increase quality without creating procedural drag?
- Could another agent follow the skills without hidden conversational context?

Only concrete defects found during these projects should reopen the ten foundational skills.

---

## Milestone 5 — Repository Tooling

**Status:** Planned after real-world validation begins

Add lightweight validation tooling where it provides repeatable value.

Potential tools:

- YAML-frontmatter validation;
- required-section checks;
- unresolved placeholder detection;
- broken repository-relative link detection;
- stale installed-skill reference detection;
- conditional-reference checks for nonexistent skills;
- duplicate ownership and responsibility-language checks;
- Markdown fence validation;
- skill inventory generation.

Tooling should validate structure and consistency. It must not pretend to automatically evaluate subjective
creative quality.

---

## Milestone 6 — Documentation and Examples

**Status:** Ongoing

Expand repository documentation with:

- architecture overview;
- skill-routing examples;
- product-led, creative-led, and mixed workflow diagrams;
- a guide for adding a new skill;
- examples of bounded versus material decisions;
- examples of creative-conformance findings;
- example task frames and handoff packages;
- guidance for adapting the skills to different agents;
- a glossary of recurring authority terms.

Where useful, add compact examples under:

```text
.agents/skills/<skill-name>/examples/
```

Examples should demonstrate the skill without becoming mandatory fictional product domains.

---

## Milestone 7 — Versioning and Release

**Status:** Planned

Before declaring a stable release:

- validate all skills;
- complete at least one project from each validation category;
- document known limitations;
- establish a versioning policy;
- add contribution guidance;
- define how breaking authority changes are identified;
- tag the first stable release.

A breaking change includes:

- changing which skill owns a responsibility;
- changing conflict-resolution priority;
- changing material-decision authority;
- changing required handoff order;
- changing activation in a way that materially alters project routing.

---

## Milestone 8 — Capability-Layer Integration

**Status:** Active

The capability layer was imported in bulk and integrated only partially. `skills-assessment-report.md` contains
the full evidence; this milestone tracks the work.

### Completed

- Framework/capability class distinction defined in `skill-authoring-standard`, with class-scoped validation
  gates and a Capability Skill Integration Contract.
- `web-design/` routing document created, covering all 82 skills with precedence rules; the stale twelve-skill
  inventory in `WEB-DESIGN-SKILLS.md` superseded rather than maintained in parallel.
- Delegated Creative Authority formalized in `task-framing` and recognized by `creative-direction` and
  `visual-design`, without extending creative origination to implementation disciplines.
- Direct contradictions resolved: shadcn component-mechanics versus authored page design; the superseded local
  `gsap` skill versus the current `gsap-*` family; smooth-scroll engine selection; `landing-page` and
  `pricing-page` versus `copywriting`/`product-design` ownership; `design-first-ui-prompting` false activation
  on real interface work.

### Outstanding, in priority order

1. **Reduced-motion coverage.** 36 of 82 `web-design/` skills produce animation with no `prefers-reduced-motion`
   path, so their output fails `frontend-development`'s own validation gate. The contract now requires it; the
   existing skills do not yet satisfy it.
2. **Empty sections.** 26 skills contain an empty `## Workflow` heading. Mechanical, but each is a skill whose
   procedure was never written.
3. **Direction-skill quality.** The direction family specifies its targets in adjectives rather than constants.
   Rewrite to carry real values (scale, tracking, spacing, radii, motion timing) before consolidating — the
   routing document currently marks them as mood references, not specifications.
4. **Consolidation.** Classify the direction skills by actual mechanism overlap, separate true directions from
   techniques mis-filed as directions, and merge only where semantic overlap is verified. The family groupings
   in the routing document are a hypothesis for this work, not an approved partition.
5. **Duplicate clusters.** `masked-reveal`/`staggered-word-reveal`; the scroll-storytelling group; the framed-
   container group. Routed around for now, not resolved.
6. **Subtype authoring standards.** `direction`, `workflow`, and `foundation` subtypes have no body standard.
   Author them from evidence once the rewrite work above shows what those skills actually need.
7. **Provenance and portability.** Strip project-specific names and hard-coded paths (`Tidal Commons`,
   `Vesperfall`, `articles/YYYY-MM-DD-*`) per `AGENTS.md` §13.
8. **Off-mission families.** `game-development/` (20 skills, internally coherent and well-routed),
   React Native, Apple-platform profiling, TTS, and personal social-content workflows are outside this
   repository's stated scope and add activation noise. Relocation — not deletion — pending a decision.

### Candidate capability gaps

Recorded from the assessment; **not** approved for creation. Adding skills to a partially-integrated collection
would worsen the selection problem this milestone exists to fix. Re-evaluate after the outstanding work above.

- `typographic-systems` — no installed skill covers modular scale ratios, measure, tracking compensation at
  display sizes, or font-loading strategy.
- `colour-systems` — no installed skill covers ramp construction or contrast computation; exactly one file in
  the repository states a numeric contrast ratio.
- `web-performance-budgets` — `frontend-development` requires performance evidence but refuses to invent
  budgets, and no skill supplies them, making the requirement unsatisfiable by default.
- `rendered-design-review` — craft-level QA (optical alignment, radius nesting, scale integrity at narrow
  widths) is owned by neither `creative-direction`'s rendered review nor `testing-and-verification`.

---

## Future skills — evidence required

These are possible future capabilities, not commitments:

- `backend-development`
- `database-development`
- `infrastructure-development`
- `motion-development`
- deployment or release management
- security verification
- performance engineering
- content strategy
- research or reference-sourcing specialists

A new skill should be added only when:

1. repeated work exposes a distinct capability gap;
2. the responsibility cannot be cleanly handled by an existing skill;
3. it has a unique procedure, evidence model, and authority boundary;
4. keeping it separate reduces ambiguity rather than adding ceremony.

---

## Non-goals

The project is not intended to become:

- a large prompt collection with overlapping instructions;
- a mandatory waterfall process;
- a framework that routes every website through product design;
- an excuse to generate planning documents for trivial tasks;
- a replacement for direct user decisions;
- a guarantee of creative quality through checklist compliance;
- an agent-specific wrapper tied permanently to one model or IDE;
- a system that treats more skills as inherently better.

The framework should remain composable, evidence-driven, and capable of moving quickly when the task is already
clear.

---

## Current execution order

```text
1. Complete Milestone 8's outstanding capability-layer integration work, in the listed priority order.
2. Run the three real-world validation project categories through the complete framework.
3. Capture authored-interaction and motion evidence during those frontend implementations, including whether
   Delegated Creative Authority resolves the exact-choreography seam in practice.
4. Fix only concrete defects exposed by actual use.
5. Re-evaluate the candidate capability gaps against evidence from that use.
6. Add lightweight validation tooling after the first validation work demonstrates which checks are worth
   automating.
7. Expand documentation and examples from validated workflows.
8. Prepare the first stable release after the roadmap's release gates are satisfied.
```
