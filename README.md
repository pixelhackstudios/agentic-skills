# Agentic Skills

Composable skills for AI coding agents working on web products and digital experiences — a governance framework
covering task scoping, product design, creative direction, visual design, UX writing, promotional copywriting,
implementation, and independent verification, plus a capability layer supplying the craft, technique, and
library knowledge that framework executes with.

Each skill is a self-contained `SKILL.md` file under `.agents/skills/<name>/`. Agents configured to discover
`SKILL.md` files under `.agents/skills/` can consume these directly without a separate build step.

## Two classes of skill

[`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) defines two skill classes with
different structural requirements, because they answer different questions.

**Framework skills** answer *who decides what, on what evidence, and where does the work go next.* Ten are
installed (listed below). They carry the full structure: explicit `Trigger immediately for` / `DO NOT trigger
for` conditions, a defined authority boundary, an ordered procedure, evidence requirements, validation gates,
and a conflict-resolution hierarchy. Installing or changing the ownership of one is a material change.

**Capability skills** answer *how is this actually made.* They supply technique, library knowledge, visual
direction, and reusable workflows, and they execute within the authority framework skills define. They satisfy
a minimum [integration contract](.agents/skills/skill-authoring-standard/SKILL.md) — identity, discriminating
activation description, capability type, boundary against the nearest neighbouring skill, framework authority
boundary, provenance, and motion lifecycle where relevant — plus their subtype's own body standard. They are
deliberately **not** required to carry the twelve framework sections; a technique skill wrapped in governance
ceremony buries the content that changes agent behaviour.

Capability skills are selected through their family's routing document, not by description matching alone:

| Family | Routing document |
|---|---|
| Web design, motion, 3D, layout, interaction technique | [`web-design/README.md`](.agents/skills/web-design/README.md) |
| Three.js/browser game development | [`game-development/README.md`](.agents/skills/game-development/README.md) |
| GSAP | [`gsap-core`](.agents/skills/gsap-core/SKILL.md) and its `Related skills` links |
| React/Next.js engineering, shadcn/ui, Vercel guidance | Loaded directly by name; no family router required |

Several capability families were imported and are still being brought up to the integration contract; see
`ROADMAP.md` for current status and `skills-assessment-report.md` for the findings driving that work.

## Why this exists

Disciplined execution of individual skills (don't invent product behaviour in visual design, don't write final
copy in product design, don't fabricate claims in promotional copy, don't skip verification evidence) is
necessary but not sufficient — it can still produce
a competent, correctly-behaved, but completely generic result: another SaaS dashboard, another card grid,
another shadcn-default composition. This framework adds a layer specifically to prevent that: `creative-direction`
owns the expressive thesis and cross-disciplinary coherence that makes a product or site feel authored and
specific rather than an interchangeable assembly of familiar patterns, without giving up rigor, evidence, or
authority boundaries anywhere else.

It's also deliberately not SaaS-first. Ordinary application/product work and public, expressive, editorial, or
campaign web work are treated as two legitimate, coexisting routes, not one default with the other bolted on.

## Installed framework skills

| Skill | Owns |
|---|---|
| [`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) | The meta-standard governing how every other skill is structured and validated. |
| [`task-framing`](.agents/skills/task-framing/SKILL.md) | Scoping incoming requests into a bounded task frame and routing to the correct downstream discipline — including which of the two routes below applies. |
| [`product-design`](.agents/skills/product-design/SKILL.md) | Product behaviour, user workflows, information architecture, states, and product-level acceptance criteria. |
| [`creative-direction`](.agents/skills/creative-direction/SKILL.md) | The approved expressive thesis, anti-references, signature devices, and cross-disciplinary coherence rules that make an experience specific and authored. |
| [`visual-design`](.agents/skills/visual-design/SKILL.md) | Visual hierarchy, composition, typography, colour, spacing, component appearance, and responsive/state treatment. |
| [`ux-writing`](.agents/skills/ux-writing/SKILL.md) | Exact interface language — labels, errors, validation, confirmations, empty states — within approved behaviour and voice. |
| [`copywriting`](.agents/skills/copywriting/SKILL.md) | Promotional, persuasive, campaign, editorial, and public-facing brand-voice language — headlines, hero statements, narrative, and CTAs — within approved creative direction. |
| [`software-development`](.agents/skills/software-development/SKILL.md) | Implementing approved, bounded software changes with scope discipline and implementation self-validation evidence; supplies the governing implementation baseline `frontend-development` operates under. |
| [`frontend-development`](.agents/skills/frontend-development/SKILL.md) | Specialist frontend implementation — markup, styles, components, responsive/client-side behaviour, server-rendered frontend concerns, accessibility mechanics, performance, and approved motion — activating jointly with `software-development`. |
| [`testing-and-verification`](.agents/skills/testing-and-verification/SKILL.md) | Independent, evidence-backed verification of implementation claims, defects, and acceptance criteria. |

## How work routes through the skills

`task-framing` picks the route based on the task's dominant purpose — it does not default to a product-design-led
pipeline merely because the deliverable is a website.

```text
Interactive product / application
task-framing → product-design → creative-direction (when activated) → visual-design + ux-writing
    → software-development baseline + frontend-development specialist execution → testing-and-verification
      (when independently required)

Expressive / editorial / campaign / portfolio / public website
task-framing
    ├─ creative-direction, for the overall expressive concept
    └─ product-design, only where workflows, IA, forms, transactions, permissions, or interaction-state
       logic require definition
           ↓ (both branches feed forward)
    applicable visual-design / ux-writing / copywriting outputs (not every surface requires all three —
    a purely editorial page with no interface controls may need no ux-writing at all)
    → software-development baseline + frontend-development specialist execution → testing-and-verification
      (when independently required)

Mixed website (e.g. a campaign page with one signup form)
task-framing
    ├─ creative-direction, for the overall expressive concept
    └─ product-design, narrowly, only for the interactive element's behaviour
           ↓ (both branches feed forward)
    visual-design + ux-writing/copywriting consume the applicable approved outputs
    → software-development baseline + frontend-development specialist execution → testing-and-verification
      (when independently required)
```

`frontend-development` activates jointly with `software-development` for any production change to a frontend
implementation surface, regardless of visual ambition — `software-development` supplies the governing baseline
(scope, classification, evidence honesty); `frontend-development` supplies frontend-specific execution and
self-validation within it. A bounded frontend task may also proceed directly from the task frame's requirements
and established conventions when no upstream product-design, creative-direction, or visual-design package is
necessary. Independent verification by `testing-and-verification` is required when the task frame, risk, or
acceptance criteria call for it; otherwise self-validated work returns to the current workflow.

`creative-direction` grounds itself in an approved `product-design` specification when one exists, or directly in
the task's audience, communication objective, content requirements, and user-approved references when it doesn't
(public/expressive work with no product-design specification to consume).

**Where capability skills enter.** They are not a stage in this pipeline — they are consulted *within* stages.
`creative-direction` and `visual-design` consult direction and technique skills while forming and specifying the
work; `frontend-development` consults technique and library skills while implementing it. A capability skill
never overrides an approved specification or thesis, and selecting one is a bounded decision unless it forces a
material one (a new dependency, a new token system, a new interaction model).

**When the user delegates creative control.** An explicit grant — "you have creative control", "make it
experimental" — is recorded by `task-framing` as a Delegated Creative Authority envelope inside the frame's
authorized decisions. It converts named expressive categories (signature devices, motion character, type system,
palette, layout paradigm) from escalations into authorized bounded decisions for `creative-direction` and
`visual-design`. It does not move ownership downstream: `frontend-development` gains no authority to originate
creative concepts, and product behaviour, accessibility scope, public claims, pricing, architecture, and
performance policy continue to escalate regardless.

## Status

- **Framework layer:** ten skills installed; all ten pass `skill-authoring-standard`'s framework-skill
  structural validation (required sections, frontmatter, anchors). No known structural compliance gaps remain.
- **Capability layer:** 137 skills installed across `web-design/`, `game-development/`, `codex/`, `gsap-*`,
  `vercel-*`, `shadcn`, `ui/`, and `media/`. These were imported in bulk and are being brought up to the
  Capability Skill Integration Contract incrementally. The `web-design/` family is routed
  ([`web-design/README.md`](.agents/skills/web-design/README.md)); several known defects remain, catalogued in
  `skills-assessment-report.md` and tracked in `ROADMAP.md`. Do not read "installed" as "conformant" for this
  layer.
- **Delegated Creative Authority** is formalized in `task-framing` and recognized by `creative-direction` and
  `visual-design`: an explicit user grant of creative latitude converts named expressive categories into
  authorized bounded decisions instead of escalations, without relocating ownership to implementation
  disciplines and without covering product behaviour, accessibility scope, public claims, or architecture.
- Structural validation is not the same as real-world validation. Real-world validation across the project
  categories in `ROADMAP.md` is the active phase and is not complete.
- Motion implementation remains inside `frontend-development`, and the authority over *exact* authored
  choreography remains a deliberately open seam; whether it warrants an independent `motion-development` skill
  is evaluated through real implementation evidence, not resolved in advance.

## Using these skills

Copy `.agents/skills/` into a project's root. Each `SKILL.md`'s frontmatter (`Trigger immediately for` /
`DO NOT trigger for`) tells a skill-aware agent when to load it. `skill-authoring-standard` governs how to add,
modify, or audit any skill in this directory — read it before changing the structure of an existing skill or
adding a new one.

**How capability skills get selected.** Family routing documents are not loaded by skill discovery, so nothing
would consult them on their own. The path runs through the framework layer instead: `creative-direction`,
`visual-design`, and `frontend-development` each carry a `Consuming Capability Skills` section requiring
router consultation before selecting a capability skill, and a validation gate checking it. Those three activate
from their own frontmatter conditions, so the router enters context as a consequence of normal discipline
activation rather than needing to be found. `frontend-development` activates for any production frontend change,
which makes it the backstop: a request that skips design entirely still passes through the router on its way to
implementation.

This means the framework layer is load-bearing for capability selection. If you adopt `.agents/skills/`
selectively and omit the framework skills, add the equivalent instruction to your own workspace rules — consult
the applicable family routing document before choosing among capability skills — or the capability layer will
be selected by description match, which its overlapping vocabulary does not support.

This repository's root `AGENTS.md` governs maintenance of the Agentic Skills framework itself — it is not
automatically the operating contract for a project that consumes these skills. A host project should supply its
own workspace rules appropriate to that project; the reusable package remains `.agents/skills/`.
