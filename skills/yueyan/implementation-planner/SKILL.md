---
name: implementation-planner
description: Turn reviewed MVP blueprints, PRDs, requirement documents, feature specs, or validated product plans into engineering-ready implementation plans before coding. Use when Codex needs to freeze blocking decisions, define system boundaries, separate manual backstops from system capabilities, design entities/states/interfaces/modules, sequence implementation slices, write acceptance criteria, or identify build-blocking gaps after requirement review is complete.
---

# Implementation Planner

Turn a reviewed product blueprint into an implementation plan that engineering can execute.

The goal is not to rewrite the requirements or start coding immediately. The goal is to stop engineering from guessing by freezing system boundaries, implementation order, interface contracts, acceptance criteria, and blocking gaps.

## Core Goal

Produce an implementation plan that lets engineering break work down without guessing. At minimum, make all of the following clear:

- what the system must actually do in v1
- which steps remain `manual backstop`
- which facts are still unset and must be frozen first
- how to define the core entities, states, interfaces, and modules
- what order implementation should follow and which minimum slice should be built first
- what outcome counts as a real v1 delivery

## Hard Rules

- Read the source materials first. If the request references an existing blueprint, review, PRD, codebase, workflow document, or ticket, inspect local context before asking questions.
- Treat `requirement excavation` and `review` outputs as inherited constraints by default. Do not reopen product direction unless the inputs conflict.
- Do not smuggle a more ambitious future architecture into the v1 plan.
- Distinguish clearly between `system capability`, `manual backstop`, and `non-goal`.
- Distinguish clearly between `artifact fact`, `default assumption`, and `to confirm`.
- Put every implementation-blocking unknown near the front of the plan. Do not bury them at the end.
- Do not invent external data sources, permissions, integrations, SLAs, throughput assumptions, or business rules that the user did not provide.
- If the prior review already cut scope, preserve that cut. Do not add it back implicitly.
- Ask at most 2 questions per round, and only ask facts that block implementation design.
- If the input has not been reviewed yet, say so first and produce a conservative plan.

## Default Workflow

1. Read the input materials and extract the confirmed `target user`, `core loop`, `must-have`, `manual backstop`, `non-goal`, and `success signal`.
2. List the unresolved facts that would block engineering.
3. Freeze the minimum implementation boundary. Do not turn this into roadmap planning.
4. Translate the requirements into entities, states, modules, interfaces, execution flow, and exception branches.
5. Break implementation into slices by dependency order, not by feature wishlist.
6. Produce acceptance criteria, risks, open questions, and the first build slice.

## Phase 0: Read and Inherit

Before planning, confirm whether the materials already define:

- who the first user is
- what the core loop is
- what has already been removed from v1
- which steps allow manual backstop
- what the prior review identified as the main risks
- whether codebase, database, interface, or runtime constraints already exist

If the request continues from an existing repo, service, or scaffold, inspect the relevant local files first before deciding whether to ask the user anything.

## Phase 1: Freeze Blocking Decisions

Goal: identify the facts that must be decided before engineering can design interfaces or sequence tasks.

Typical blocking decisions include:

- input source and input format
- output shape and delivery location
- execution mode: scheduled, manual, event-driven, synchronous, or asynchronous
- single-user or multi-user
- single-object or multi-object
- language, locale, timezone, or formatting constraints
- external dependencies, budget, permissions, or compliance constraints
- degradation strategy and failure-handling boundaries

If these facts are missing:

- list `blocking gaps` first
- then list `default assumptions`
- only plan on top of assumptions when doing so will not mislead engineering

## Phase 2: Define System Boundary

Goal: translate the requirements into engineering objects instead of repeating product vision.

Make these explicit:

- `system capability`: what the system must do itself
- `manual backstop`: which steps may still be handled by humans
- `non-goal`: what v1 explicitly does not do

Then define:

- roles
- core entities or objects
- key states
- state transitions
- input validation
- output contracts
- external dependency boundaries

If a critical step fundamentally depends on human judgment, do not pretend it is automation. Put it under `manual backstop`.

## Phase 3: Design the Execution and Exception Flow

Goal: make it clear how the system gets from input to output.

Cover at least:

- trigger
- input intake
- normalization or validation
- decision logic
- action execution
- persistence or state update
- output generation
- failure branch
- degradation branch

The point is not to produce a pretty flowchart. The point is to answer:

- what input each step consumes
- what decision each step makes
- what state each step writes or changes
- how each step degrades on failure
- which failures are tolerable and which must stop the run

## Phase 4: Plan Modules and Interfaces

Goal: define engineering boundaries so implementation does not become improvisation.

Prioritize:

- core module or service boundaries
- interfaces between modules
- configuration structure
- data model or object model
- scheduling or entrypoint layer
- logs, observability, audit, or state-recording points

Module boundaries should serve implementation order. Do not over-layer the design in the name of architecture purity.

If an existing codebase or framework already exists, fit the plan to the existing boundaries instead of dropping in an idealized architecture.

## Phase 5: Sequence the Implementation Slices

Goal: break work into the smallest buildable delivery path according to dependency order.

By default, decompose implementation into these slice types:

1. skeleton slice
2. input slice
3. core logic slice
4. output slice
5. failure and degradation slice
6. validation and observability slice

For each slice, state:

- goal
- prerequisite dependencies
- main surface area of change
- done definition

Prefer the order that gets the minimum closed loop running fastest. Do not start with visualization, optimization, permissions, or secondary features.

## Phase 6: Acceptance and Open Questions

Goal: end the habit of shipping based on "this seems roughly done."

Always output:

- minimum acceptance criteria
- explicit non-goals
- risks
- `to confirm`
- `default assumptions`
- `blocking gaps`

Acceptance criteria must be observable, testable, and decidable. Do not write:

- better user experience
- good output quality
- more stable system

Rewrite them into conditions such as:

- given valid input, the system produces the expected output
- when input is missing, the system returns an understandable error or degraded result
- key states are recorded and traceable
- the user can complete the minimum closed loop without engineering explanation

## Common Planning Failures

Call these out directly when they appear:

- reopening product debates during implementation planning
- discussing technical options before freezing inputs and outputs
- quietly converting manual steps into automated capabilities
- decomposing tasks by pages, buttons, or feature labels instead of dependencies and loop structure
- documenting only the happy path and ignoring failure or degradation
- listing modules without interface contracts
- listing dev tasks without acceptance criteria
- reintroducing future concerns such as performance tuning, extensibility, multi-tenant architecture, or recommendation systems into v1

## Question Style

Ask only for facts that block implementation. Keep questions short, hard, and answerable.

Prefer:

- "Where does the final output land?"
- "Which step must be systemized in v1, and which step may remain manual?"
- "If the external dependency fails, does v1 stop, retry, or degrade?"
- "What is the primary data object here?"
- "What is the done definition for this step?"

Avoid:

- "What do you hope this becomes later?"
- "What other features do you want?"
- "What other possibilities should we consider?"

## Output: Implementation Plan

Use this structure unless the user explicitly asks for a different format:

```md
# Implementation Plan

## 1. Planning Basis
- Source artifacts:
- Inherited scope:
- Review constraints:

## 2. Blocking Decisions
- To confirm:
- Default assumptions:
- Blocking gaps:

## 3. System Boundary
- System capabilities:
- Manual backstops:
- Explicit non-goals:

## 4. Domain Model
- Roles:
- Core entities or objects:
- Key states:
- State transitions:

## 5. Interfaces and Contracts
- Inputs:
- Input sources:
- Input validation:
- Outputs:
- Output destination:
- External dependencies:

## 6. Execution Flow
1. Trigger:
2. Input intake:
3. Validation and normalization:
4. Decision logic:
5. Action execution:
6. State updates:
7. Failure handling:
8. Degradation handling:
9. Output generation:

## 7. Module Breakdown
- Module:
- Responsibility:
- Key interfaces:
- Notes:

## 8. Delivery Sequence
### Slice 1
- Goal:
- Depends on:
- Done when:

### Slice 2
- Goal:
- Depends on:
- Done when:

### Slice 3
- Goal:
- Depends on:
- Done when:

## 9. Acceptance Criteria
- Minimum acceptance criteria:
- Error and degradation criteria:
- Observability criteria:

## 10. Risks and Open Questions
- Risks:
- To confirm:
- Default assumptions:
- Blocking gaps:

## 11. First Build Slice
- Recommended starting point:
- Why this comes first:
- What v1 still does not include:
```

## Decision Heuristics

Use these defaults:

- If the input boundary is unclear, freeze blocking decisions before pretending module design can proceed.
- If system capability and manual work are mixed together, separate the boundary before decomposing tasks.
- If the exception flow is unclear, define failure and degradation strategy before refining interfaces.
- If the task order does not produce a minimum closed loop, reorder the slices.

## Success Standard For This Skill

The plan produced by this skill must:

- let engineering start task breakdown instead of guessing requirements
- make implementation-blocking unknowns explicit
- make the v1 system boundary and non-goals explicit
- translate interfaces, states, and modules into code-shape decisions
- provide a defensible implementation order instead of a flat feature list
- provide completion criteria that can actually be accepted
