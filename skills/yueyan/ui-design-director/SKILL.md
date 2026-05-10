---
name: ui-design-director
description: Orchestrate frontend-skill and Impeccable for UI design, redesign, implementation, audit, and polish. Use when Codex needs a repeatable workflow for landing pages, websites, dashboards, app screens, product UI, design-system cleanup, visual QA, responsive hardening, or when the user asks how to mix lightweight frontend craft rules with Impeccable's teach/document/shape/craft/audit/polish/harden process.
---

# UI Design Director

Blend two complementary UI skills:

- `frontend-skill` is the default taste layer: composition, hierarchy, first viewport, copy restraint, imagery, motion, responsive fit, and avoiding generic card-heavy UI.
- `impeccable` is the design operating system: product context, design documentation, shape briefs, craft gates, audits, polish, and hardening.

Use `frontend-skill` for everyday UI work. Use Impeccable at project start, before major design decisions, and before shipping important surfaces.

## Operating Rule

Decide the depth before touching code:

- **Tiny UI fix**: use `frontend-skill` only.
- **Normal feature UI**: use `frontend-skill`, then screenshot-check the result.
- **Important page or redesign**: use Impeccable for context and direction, then implement with `frontend-skill` rules active.
- **Pre-release quality pass**: use Impeccable `audit`, `polish`, and `harden`, then verify in browser across desktop and mobile.

If both skills conflict, prefer the project-specific Impeccable context from `PRODUCT.md` and `DESIGN.md`, while keeping `frontend-skill` as the practical guardrail against bland, crowded, or fragile UI.

## First Project Setup

When a project has no durable product/design context:

1. Run `$impeccable teach` or help the user create the equivalent `PRODUCT.md`.
2. Capture users, product purpose, brand tone, register (`brand` or `product`), reference sites, anti-references, and strategic design principles.
3. If the project already has meaningful UI, run `$impeccable document` or create the equivalent `DESIGN.md`.
4. Continue only after the context is specific enough to prevent generic output.

Do not use Impeccable `craft` before a user-confirmed shape brief unless the user already gave a confirmed direction.

## New UI Or Redesign Workflow

Use this for landing pages, homepages, dashboards, onboarding, settings, app shells, or high-visibility components.

1. Load or establish Impeccable context.
2. Classify the surface:
   - **brand**: marketing, landing, campaign, portfolio, editorial, venue, product showcase.
   - **product**: dashboard, admin, workspace, tool, settings, onboarding, operational UI.
3. Run or simulate `$impeccable shape`:
   - visual thesis
   - page or screen structure
   - typography/color/motion direction
   - image or asset plan
   - risks and anti-patterns
4. Ask for confirmation when the change is large or aesthetic direction is unsettled.
5. Implement with `frontend-skill` constraints:
   - brand/product is visible in the first viewport
   - no generic SaaS card grid by default
   - product surfaces start with the working surface, not a marketing hero
   - copy is utility-first for tools and concise for brand pages
   - layout has stable dimensions and no text overflow
   - motion improves hierarchy or affordance
6. Verify with browser screenshots on desktop and mobile when a runnable UI exists.

## Everyday UI Development

Use this lighter loop when the user asks for ordinary UI implementation and has not requested a full redesign:

1. Read the current UI patterns before editing.
2. Apply `frontend-skill` as the baseline craft standard.
3. Keep changes scoped to the requested surface.
4. Prefer existing components, tokens, icons, routes, and layout conventions.
5. Improve obvious responsive, spacing, empty-state, and copy issues encountered in the touched area.
6. Run the app and inspect screenshots when feasible.

Do not require Impeccable preflight for small fixes unless the user explicitly invokes `$impeccable` or the surface is strategically important.

## Pre-Release Pass

Before treating important UI work as done:

1. Run or simulate `$impeccable audit` for hierarchy, accessibility, cognitive load, consistency, and implementation risks.
2. Run or simulate `$impeccable polish` for spacing, typography, color, alignment, microcopy, and interaction detail.
3. Run or simulate `$impeccable harden` for:
   - mobile and wide desktop
   - long text and i18n expansion
   - loading, empty, error, and disabled states
   - keyboard and focus behavior
   - reduced-motion expectations
4. Use browser screenshots or visual inspection to verify the actual rendered result.
5. Fix concrete issues instead of only reporting them, unless the user asked for review only.

## Output Style

When using this skill, report the chosen workflow briefly:

```text
UI_DIRECTOR: depth=<tiny|normal|important|pre-release> frontend-skill=<used|not-needed> impeccable=<teach|document|shape|craft|audit|polish|harden|skipped:reason>
```

For important work, include the shape direction before implementation. For review work, lead with findings and concrete fixes.

## Anti-Patterns

- Do not run a heavyweight Impeccable flow for a button alignment tweak.
- Do not skip product context for a major redesign.
- Do not turn product dashboards into marketing landing pages.
- Do not accept a beautiful static mock if the real rendered page has overflow, broken mobile layout, weak contrast, or missing states.
- Do not let `frontend-skill` produce a generic full-bleed brand page when Impeccable context says the surface is a dense product workspace.
