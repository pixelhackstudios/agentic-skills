# Agentic Skills

A set of composable governance skills for AI coding agents working on web products and digital experiences — from
initial task scoping through product design, creative direction, visual design, UX writing, implementation, and
independent verification.

Each skill is a self-contained `SKILL.md` file under `.agents/skills/<name>/`, following the structure defined by
[`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md): YAML frontmatter with explicit
`Trigger immediately for` / `DO NOT trigger for` conditions, a defined authority boundary, an ordered procedure,
evidence requirements, validation gates, and a conflict-resolution hierarchy. Agents configured to discover
`SKILL.md` files under `.agents/skills/` can consume these directly without a separate build step.

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

## Installed skills

| Skill | Owns |
|---|---|
| [`skill-authoring-standard`](.agents/skills/skill-authoring-standard/SKILL.md) | The meta-standard governing how every other skill is structured and validated. |
| [`task-framing`](.agents/skills/task-framing/SKILL.md) | Scoping incoming requests into a bounded task frame and routing to the correct downstream discipline — including which of the two routes below applies. |
| [`product-design`](.agents/skills/product-design/SKILL.md) | Product behaviour, user workflows, information architecture, states, and product-level acceptance criteria. |
| [`creative-direction`](.agents/skills/creative-direction/SKILL.md) | The approved expressive thesis, anti-references, signature devices, and cross-disciplinary coherence rules that make an experience specific and authored. |
| [`visual-design`](.agents/skills/visual-design/SKILL.md) | Visual hierarchy, composition, typography, colour, spacing, component appearance, and responsive/state treatment. |
| [`ux-writing`](.agents/skills/ux-writing/SKILL.md) | Exact interface language — labels, errors, validation, confirmations, empty states — within approved behaviour and voice. |
| [`copywriting`](.agents/skills/copywriting/SKILL.md) | Promotional, persuasive, campaign, editorial, and public-facing brand-voice language — headlines, hero statements, narrative, and CTAs — within approved creative direction. |
| [`software-development`](.agents/skills/software-development/SKILL.md) | Implementing approved, bounded software changes with scope discipline and validated evidence. |
| [`testing-and-verification`](.agents/skills/testing-and-verification/SKILL.md) | Independent, evidence-backed verification of implementation claims, defects, and acceptance criteria. |

## How work routes through the skills

`task-framing` picks the route based on the task's dominant purpose — it does not default to a product-design-led
pipeline merely because the deliverable is a website.

```text
Interactive product / application
task-framing → product-design → creative-direction (when activated) → visual-design + ux-writing
    → software-development → testing-and-verification

Expressive / editorial / campaign / portfolio / public website
task-framing
    ├─ creative-direction, for the overall expressive concept
    └─ product-design, only where workflows, IA, forms, transactions, permissions, or interaction-state
       logic require definition
           ↓ (both branches feed forward)
    visual-design + ux-writing + copywriting
    → software-development → testing-and-verification

Mixed website (e.g. a campaign page with one signup form)
task-framing
    ├─ creative-direction, for the overall expressive concept
    └─ product-design, narrowly, only for the interactive element's behaviour
           ↓ (both branches feed forward)
    visual-design + ux-writing/copywriting consume the applicable approved outputs
    → software-development → testing-and-verification
```

`creative-direction` grounds itself in an approved `product-design` specification when one exists, or directly in
the task's audience, communication objective, content requirements, and user-approved references when it doesn't
(public/expressive work with no product-design specification to consume).

## Status

- Nine foundational skills installed and stable as of this writing — no further architectural changes unless
  real use surfaces a concrete defect.
- The next identified gap is specialist frontend implementation — particularly authored interaction and motion —
  which is currently handled generically by `software-development`.

## Using these skills

Copy `.agents/skills/` into a project's root. Each `SKILL.md`'s frontmatter (`Trigger immediately for` /
`DO NOT trigger for`) tells a skill-aware agent when to load it. `skill-authoring-standard` governs how to add,
modify, or audit any skill in this directory — read it before changing the structure of an existing skill or
adding a new one.
