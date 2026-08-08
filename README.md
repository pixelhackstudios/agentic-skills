# Agentic Skills

Composable skills that give AI coding agents a working discipline for web design and development — who decides
what, on what evidence, and with what craft.

Each skill is a self-contained `SKILL.md` under `.agents/skills/<name>/`. Agents that discover `SKILL.md` files
there consume them directly; there is no build step.

> ### Goal
>
> **Reliably improve capable frontier models toward award-caliber web design and development quality.**
>
> ### Current status
>
> Agentic Skills is architecturally integrated and ready for empirical validation. **It has not yet demonstrated
> that it improves web-design quality or reaches an award-caliber bar.** Those claims are now governed by the
> frozen [`REAL-WORLD-VALIDATION-PROTOCOL.md`](REAL-WORLD-VALIDATION-PROTOCOL.md) and will be made only if the
> benchmark evidence supports them.
>
> - **V1 under test:** commit `efa22b9` — frozen, no architecture changes without empirical evidence
> - **Protocol v1.0:** commit `2e6ddf8` — pre-registered, methodology frozen
> - **Next:** Tranche 0 harness validation → first three benchmark briefs → anchor set → scored benchmark

---

## The problem this exists to solve

A capable model given a website brief will usually produce something competent and completely interchangeable:
another SaaS dashboard, another card grid, another shadcn-default composition with a gradient blob behind it.
Not wrong — just anonymous. It reads as output rather than as work someone made.

Skill discipline alone does not fix this. An agent can correctly refuse to invent product behaviour, correctly
avoid fabricating claims, correctly gather verification evidence — and still ship something generic, because
none of those rules are about whether the result is *specific to this problem*.

Agentic Skills addresses that directly. [`creative-direction`](.agents/skills/creative-direction/SKILL.md) owns
an expressive thesis and cross-disciplinary coherence rules that downstream work must conform to, with an
operational anti-generic standard that names observable failure patterns rather than gesturing at "quality."
The capability layer supplies the craft to execute on it. The rest of the framework keeps that ambition honest —
bounded scope, explicit authority, evidence for every completion claim.

It is deliberately not SaaS-first. Application work and public/expressive/editorial work are two legitimate
routes, not one default with the other bolted on.

## Two classes of skill

[`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) defines two classes, because they
answer different questions and need different structures.

**Framework skills — *who decides what?*** Ten installed. Each owns a discipline's authority boundary,
activation conditions, decision classification, escalation path, and handoffs. They carry the full structure:
explicit trigger/non-trigger conditions, ordered procedure, evidence requirements, validation gates, and a
conflict-resolution hierarchy. Adding one or moving a responsibility between them is a material change.

**Capability skills — *how is this actually made?*** 137 installed. Technique, library knowledge, visual
direction, and reusable workflows, executing inside the authority framework skills define. They satisfy a
minimum integration contract — identity, discriminating activation description, capability type, boundary
against the nearest neighbouring skill, framework authority boundary, provenance, and motion lifecycle where
relevant — plus their subtype's own body standard. They are deliberately **not** required to carry the twelve
framework sections: a technique skill wrapped in governance ceremony buries the content that changes agent
behaviour.

### Framework skills

| Skill | Owns |
|---|---|
| [`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) | How every skill is classified, structured, and validated |
| [`task-framing`](.agents/skills/task-framing/SKILL.md) | Scoping a request into a bounded frame; routing; Delegated Creative Authority |
| [`product-design`](.agents/skills/product-design/SKILL.md) | Behaviour, workflows, information architecture, states, product acceptance criteria |
| [`creative-direction`](.agents/skills/creative-direction/SKILL.md) | The expressive thesis, anti-references, signature devices, cross-disciplinary coherence |
| [`visual-design`](.agents/skills/visual-design/SKILL.md) | Hierarchy, composition, typography, colour, spacing, component appearance, responsive/state treatment |
| [`ux-writing`](.agents/skills/ux-writing/SKILL.md) | Exact interface language — labels, errors, validation, confirmations, empty states |
| [`copywriting`](.agents/skills/copywriting/SKILL.md) | Public-facing promotional, campaign, editorial, and brand-voice language |
| [`software-development`](.agents/skills/software-development/SKILL.md) | The governing implementation baseline: scope discipline, change classification, evidence honesty |
| [`frontend-development`](.agents/skills/frontend-development/SKILL.md) | Specialist frontend execution — markup, styles, components, responsive behaviour, a11y mechanics, performance, approved motion |
| [`testing-and-verification`](.agents/skills/testing-and-verification/SKILL.md) | Independent, evidence-backed verification and the formal verdict |

### Capability families

Selected through a family routing document, never by description matching alone — overlapping vocabulary across
dozens of skills does not discriminate.

| Family | Count | Routing |
|---|---|---|
| Web design — motion, 3D, layout, interaction technique | 82 | [`web-design/README.md`](.agents/skills/web-design/README.md) |
| Game development — Three.js/browser games | 20 | [`game-development/README.md`](.agents/skills/game-development/README.md) |
| Workflow and tooling | 19 | [`codex/`](.agents/skills/codex/) — loaded by name |
| GSAP | 7 | [`gsap-core`](.agents/skills/gsap-core/SKILL.md) and its `Related skills` links |
| React/Next.js engineering | 5 | [`vercel-*`](.agents/skills/) — loaded by name |
| Components, media, prompting | 4 | `shadcn`, `media/`, `ui/` — loaded by name |

## How work routes

`task-framing` routes by the task's dominant purpose. It does not force a website through a product-design-led
pipeline just because it is a website.

```text
Interactive product / application
  task-framing → product-design → creative-direction (when activated) → visual-design + ux-writing
    → software-development + frontend-development → testing-and-verification (when required)

Expressive / editorial / campaign / portfolio site
  task-framing ─┬─ creative-direction — the expressive concept
                └─ product-design — only where workflows, forms, transactions, or state logic exist
                        ↓ both branches feed forward
     applicable visual-design / ux-writing / copywriting
       → software-development + frontend-development → testing-and-verification (when required)

Mixed surface (campaign page with one signup form)
  Same split, with product-design scoped narrowly to the interactive element only.
```

**Capability skills are not a pipeline stage** — they are consulted *within* stages.
`creative-direction` and `visual-design` reach for direction and technique skills while forming and specifying
the work; `frontend-development` reaches for technique and library skills while implementing it. Each carries a
`Consuming Capability Skills` section requiring router consultation, and a validation gate checking it.

```text
discipline activates → needs craft → family router → narrowest applicable skill(s)
                                                   → applied within that discipline's authority
```

A capability skill never overrides an approved thesis, specification, reference, design token, or accessibility
constraint. Selecting one is a bounded decision unless what it introduces is itself material — a new dependency,
a new token system, a new interaction model.

**When the user delegates creative control.** An explicit grant — *"you have creative control"*, *"make it
experimental"* — is recorded by `task-framing` as a **Delegated Creative Authority** envelope inside the frame's
authorized decisions, naming the receiving disciplines, the delegated categories, and the categories that stay
retained. It converts named expressive choices (signature devices, motion character, type system, palette,
layout paradigm) from escalations into authorized bounded decisions for `creative-direction` and `visual-design`,
so ambition becomes the authorized default rather than a question.

It does not move ownership. `frontend-development` gains no authority to originate creative concepts. Product
behaviour, accessibility scope, public and legal claims, pricing, architecture, and performance policy continue
to escalate regardless of how broadly creative control was granted.

## Repository map

| Document | What it is |
|---|---|
| [`README.md`](README.md) | This map |
| [`REAL-WORLD-VALIDATION-PROTOCOL.md`](REAL-WORLD-VALIDATION-PROTOCOL.md) | **Frozen v1.0.** The pre-registered experiment that governs every quality claim |
| [`ROADMAP.md`](ROADMAP.md) | Completed, active, and planned work; Milestone 8 tracks capability-layer integration |
| [`skills-assessment-report.md`](skills-assessment-report.md) | Adversarial audit of all 147 skills; the evidence driving current work |
| [`AGENTS.md`](AGENTS.md) | Operating contract for maintaining *this repository* — not for host projects |

## Using these skills

Copy `.agents/skills/` into a project root. Each skill's frontmatter tells a skill-aware agent when to load it.
Read [`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) before adding or
restructuring a skill.

**The framework layer is load-bearing for capability selection.** Family routing documents are not loaded by
skill discovery, so nothing consults them on their own. The path runs through the framework skills, which *do*
activate from their own trigger conditions and which require router consultation before selecting craft.
`frontend-development` activates for any production frontend change, making it the backstop — a request that
skips design entirely still passes through the router on its way to implementation.

If you adopt selectively and omit the framework skills, add the equivalent instruction to your own workspace
rules — *consult the applicable family routing document before choosing among capability skills* — or the
capability layer gets selected by description match, which its vocabulary does not support.

This repository's `AGENTS.md` governs maintenance of the framework itself. It is **not** automatically the
operating contract for a project consuming these skills; a host project should supply its own workspace rules.
The reusable package is `.agents/skills/`.

## Evidence status, in detail

**What is established.**

- All 10 framework skills pass `skill-authoring-standard`'s framework-skill structural validation — required
  sections, frontmatter, in-file anchors. No known structural compliance gaps.
- The capability router covers all 82 `web-design/` skills with precedence rules and nearest-neighbour
  distinctions.
- The routing path from framework activation to capability selection is wired and gated, not merely documented.
- Delegated Creative Authority is formalized in `task-framing` and recognized by `creative-direction` and
  `visual-design`, with implementation disciplines explicitly excluded.

**What is not established.**

- **That any of this produces better websites.** No benchmark has been run. Structural validation is not
  real-world validation, and the two must never be described as if one implied the other.
- **Capability-layer conformance.** 137 skills were imported in bulk; they are being brought up to the
  integration contract incrementally. Known defects — 26 empty sections, 36 animating skills with no
  reduced-motion path, adjective-driven direction skills, duplicate clusters — are catalogued in
  [`skills-assessment-report.md`](skills-assessment-report.md) and tracked in `ROADMAP.md` Milestone 8.
  **Do not read "installed" as "conformant" for this layer.**
- **Authority over exact authored choreography.** A deliberately open seam. Whether it warrants a separate
  `motion-development` skill is a question for implementation evidence, not advance argument.

**What happens next.**

1. Build and smoke-test the capture harness (protocol Tranche 0) — disposable runs, never scored.
2. Author the first three benchmark briefs, with adversarial review by a model other than the one under test.
3. Assemble the calibration anchor set.
4. Pre-register predictions and run the first scored tranche.

No further architecture changes without empirical evidence clearing the protocol's four-part threshold:
recurrence, reproduction, attribution, and a demonstrated fix that repairs the failure without regressing what
already worked.

---

*Claims about this project's output quality are constrained by `REAL-WORLD-VALIDATION-PROTOCOL.md`. Until the
benchmark runs, the honest description is: an integrated system with a credible design, an adversarial audit of
its own weaknesses, and a frozen experiment capable of proving it wrong.*
