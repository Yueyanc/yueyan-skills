---
name: requirement-excavator
description: Requirement excavation and MVP scoping for early-stage product or feature ideas. Use when the user has a vague concept and needs 需求挖掘, 需求澄清, MVP 定义, 范围收敛, target-user clarification, pain-point validation, flow definition, scope boundaries, or an engineering-ready MVP blueprint. Also use when the user wants to identify pseudo-requirements, define the smallest closed loop, decide what not to build in v1, or turn a fuzzy business idea into a buildable spec. Do not use for generic spec polishing, late-stage delivery planning, or broad strategy work without a concrete candidate loop.
---

# Requirement Excavator

Turn vague ideas into buildable MVP blueprints. The goal is not to write a polished document. The goal is to stop engineering from guessing and give them enough clarity to define entities, states, interfaces, and acceptance criteria.

## Core Goal

Find the smallest closed loop that tests the riskiest assumption first.

A good output should make all of the following clear:

- why this product deserves to exist
- what the first version must actually get working
- which steps can be handled manually at first
- what is explicitly out of scope
- what information engineering still needs before implementation

## Hard Rules

- Remove pleasantries, praise, and vague product jargon.
- Ask at most 2 questions per round.
- Do not force a three-round flow just because it exists.
- If the information is already sufficient, produce the blueprint immediately.
- If the information is insufficient, explicitly list `to confirm`, `default assumptions`, and `open gaps`.
- Do not stuff the MVP with features that may be useful later.
- Allow manual backstops in v1 if the core hypothesis can still be validated.
- Distinguish clearly between `MVP must-have`, `manual backstop`, `later validation`, and `out of scope for this phase`.
- Do not invent data sources, business rules, user roles, or external integrations that the user did not provide.
- Do not invent numeric thresholds, target volumes, time limits, or quality bars. If needed, mark them explicitly as `proposed default`, not fact.
- If more than one independent problem, audience, or solution loop appears, stop and ask the user to choose or rank one before generating a blueprint.

## Phase 0: Fast Read

Before asking questions, extract the following:

- who the first real users are
- when the need is triggered
- what blocks the user
- what result the user actually wants
- what the current workaround is and why it is not enough
- what known constraints exist
- what hypothesis this MVP is supposed to validate

If the request refers to an existing project, feature, codebase, PRD, ticket, or current workflow, inspect the relevant local context first. Use repo files, product docs, tickets, specs, and existing flows to answer what you can before questioning the user.

Only ask for gaps that cannot be resolved from available artifacts.

If most of this is already clear, skip unnecessary questions and move directly to the phase with the biggest information gap.

## Phase 1: Value and Context

Goal: determine whether this product or feature deserves v1 scope.

Ask at most 2 questions to confirm:

- who gets blocked, in what exact context, and by what problem
- how frequent and how painful the problem is
- how users solve it today and why that is insufficient

Push back on statements like:

- "a lot of people would need this"
- "anyone could use it"
- "it improves efficiency"
- "the market is huge"
- "let's just build it first"

If the user cannot explain who the first users are, when the need is triggered, and why the current substitute is not enough, do not move into solution design yet.

## Phase 2: Trigger -> Input -> Decision -> Action -> State -> Output

Goal: define the smallest business loop that the MVP must close.

Ask at most 2 questions to confirm:

- what event triggers the flow
- what input the system receives, who provides it, and what format or constraints apply
- what key decisions the system must make
- what actions the system or human operator must perform
- what states are created, updated, or completed
- what output the user sees and why it is useful
- where manual intervention is acceptable in v1 and where automation is mandatory
- how failures or exceptions should be handled in the first version

If a critical step fundamentally depends on human judgment, do not pretend it is automation. Mark it explicitly as a `manual backstop point`.

## Phase 3: Boundary and Success

Goal: freeze v1 scope and define what it means for the MVP to work.

Ask at most 2 questions to confirm:

- what seemingly reasonable features are absolutely out of scope for v1
- which steps may rely on manual backstops
- which metric or observation best proves the core hypothesis
- which constraints are non-negotiable

Reject vague success criteria such as:

- "users like it"
- "the experience feels good"
- "the feature set is complete"
- "let's launch and see"

Rewrite them into observable conditions, for example:

- a user can complete the full core flow independently
- the output does not need manual rewriting in most target cases
- a first-time user can complete the critical action within a defined time
- manual intervention stays below a defined threshold
- per-run processing time stays below a defined threshold
- there is evidence that users will keep using or paying for it

Any numeric threshold must either come from the user or be labeled explicitly as a `proposed default`.

## Challenge Rules

When any of the following appears, call it out directly:

- there is no real pain point, only feature excitement
- one MVP is trying to contain multiple products
- the core output depends on data the system cannot access
- there is no executable logic between input and output
- the success metric does not validate the core hypothesis
- the "product" is fundamentally a human service, not a software capability
- the scope is larger than what is needed to validate the hypothesis

After calling out the problem, help the user shrink it into the smallest testable loop.

If the problem is better validated as a service-first or manual-first workflow, say so explicitly and output a `Non-Software Validation Path` using the fixed template below instead of forcing a software MVP.

## Question Style

Questions should be sharp, concrete, and answerable:

- prefer facts over vision
- prefer scenarios over attitudes
- prefer constraints over imagination
- prefer "what must happen" over "what would be ideal"

If the user's answer is too broad, compress the options and offer a recommended default interpretation.

Useful question patterns:

- "Who gets blocked, at what exact moment, and by which problem?"
- "If v1 can do only one thing, what step must it complete for the user?"
- "Which step in this chain can be handled manually at first, and which must be systemized?"
- "What single result would best prove that this product deserves further investment?"

## Default Conversation Pattern

Default flow:

1. Start by naming the biggest missing fact, logic conflict, or scope inflation risk.
2. Ask no more than 2 critical questions in each round.
3. If multiple independent ideas are present, apply the ranking rule from `Hard Rules` before continuing.
4. When useful, summarize the current assumptions in 3 to 6 bullets to help converge.
5. As soon as there is enough information for a usable blueprint, stop asking and start synthesizing.
6. If key unknowns remain, list them explicitly in the blueprint rather than faking completeness.

## Stop-Asking Threshold

Stop asking and start synthesizing once all of the following are clear enough to draft a usable blueprint:

- first target user
- trigger moment
- concrete pain point
- core hypothesis
- minimum closed loop
- key inputs
- intended output
- manual backstop boundary, if any
- at least one success signal

If one of these is still missing and blocks engineering interpretation, ask the next most important question instead of outputting a falsely complete blueprint.

## Output: MVP Development Blueprint

When the information is sufficient, output an `MVP Development Blueprint` using the fixed section order below.

### 1. Core Value Definition

One sentence describing how this MVP solves the biggest pain point with the smallest possible effort.

### 2. Core Hypothesis

State clearly what hypothesis this first version is meant to validate.

### 3. Target User and Trigger Context

Explain who the first users are and in what exact situation the need appears.

### 4. Atomic Action

Define the minimum user path or minimum functional loop that the MVP must get working.

### 5. Roles, Entities, and States

List the key roles, core entities, and the most important state changes.

### 6. System Inputs and Constraints

Describe the required inputs, their sources, their formats, their constraints, and what happens when they are missing.

### 7. Non-Negotiable Constraints

List the constraints that shape the design, especially when they affect feasibility or scope. Cover time, budget, data availability, permissions, compliance, external dependencies, or operational constraints when relevant.

### 8. Process Breakdown

Describe, in order:

- trigger condition
- processing steps after input enters the system
- key decision logic
- action execution
- state transitions
- failure and exception branches
- output generation conditions

### 9. Manual Backstop Points

Make clear which steps may be handled manually in v1 and what can still be validated despite that manual handling.

### 10. Scope Boundaries

Classify items into the following 4 buckets and explain why:

- `MVP must-have`
- `manual backstop`
- `later validation`
- `out of scope for this phase`

### 11. Success Definition

Use observable, verifiable conditions to determine whether the MVP works and whether the core hypothesis is being validated.

### 12. Measurement Plan

Explain how the success signals will actually be captured. Specify what will be logged, counted, reviewed, or measured, where that information comes from, and who reviews it when relevant.

### 13. Risks and Open Questions

List logic gaps, critical dependencies, default assumptions, and unresolved risks.

### 14. Pre-Development Checklist

Provide the first engineering task brief, covering at least:

- core entities or objects
- module or interface boundaries
- input validation requirements
- state transitions
- exception handling
- minimum acceptance criteria
- what v1 explicitly does not include

## Canonical Output Template

Use this skeleton unless the user explicitly asks for a different format:

```md
# MVP Development Blueprint

## 1. Core Value Definition
<one sentence>

## 2. Core Hypothesis
- Hypothesis:
- Why this matters:

## 3. Target User and Trigger Context
- First user:
- Trigger moment:
- Pain point:
- Current workaround:

## 4. Atomic Action
- Minimum closed loop:

## 5. Roles, Entities, and States
- Roles:
- Core entities:
- Key states:

## 6. System Inputs and Constraints
- Inputs:
- Sources:
- Format and constraints:
- Missing-input handling:

## 7. Non-Negotiable Constraints
- Time constraints:
- Budget constraints:
- Data availability constraints:
- Permissions or compliance constraints:
- External dependency constraints:

## 8. Process Breakdown
1. Trigger:
2. Input intake:
3. Decision logic:
4. Action execution:
5. State transitions:
6. Failure and exception handling:
7. Output generation:

## 9. Manual Backstop Points
- Manual step:
- Why manual is acceptable in v1:
- What is still being validated:

## 10. Scope Boundaries
### MVP must-have
- ...

### Manual backstop
- ...

### Later validation
- ...

### Out of scope for this phase
- ...

## 11. Success Definition
- Observable success signals:
- Validation signal for the core hypothesis:
- User-provided metrics:
- Proposed defaults:

## 12. Measurement Plan
- What will be measured:
- How it will be captured:
- Data source or artifact:
- Review cadence or owner:

## 13. Risks and Open Questions
- Risks:
- To confirm:
- Default assumptions:
- Blocking open gaps:

## 14. Pre-Development Checklist
- Core entities or objects:
- Module or interface boundaries:
- Input validation rules:
- State transitions:
- Exception handling:
- Minimum acceptance criteria:
- Explicit v1 non-goals:
```

## Canonical Non-Software Validation Path Template

Use this skeleton when software is not the right first validation vehicle:

```md
# Non-Software Validation Path

## 1. Core Hypothesis
- Hypothesis:
- Why this should be tested before building software:

## 2. Target User and Trigger Context
- First user:
- Trigger moment:
- Pain point:
- Current workaround:

## 3. Manual Validation Flow
1. Trigger:
2. Manual intake:
3. Human decision or service step:
4. Result delivery:
5. Follow-up or feedback collection:

## 4. Non-Negotiable Constraints
- Time constraints:
- Budget constraints:
- Data availability constraints:
- Permissions or compliance constraints:
- Operational constraints:

## 5. Scope Boundaries
### Must validate now
- ...

### Manual-only for this phase
- ...

### Out of scope for this phase
- ...

## 6. Success Signals
- Observable success signals:
- Validation signal for the core hypothesis:
- User-provided metrics:
- Proposed defaults:

## 7. Measurement Plan
- What will be measured:
- How it will be captured:
- Data source or artifact:
- Review cadence or owner:

## 8. Risks and Open Questions
- Risks:
- To confirm:
- Default assumptions:
- Blocking open gaps:

## 9. Exit Criteria
- What evidence is enough to justify software:
- What evidence would stop or reshape the idea:
```

## Output Constraints

- Use engineering language, not fundraising language or vague strategy talk.
- Write descriptions so they can map to objects, states, interfaces, rules, and acceptance criteria.
- Mark uncertain points as `to confirm` or `default assumption`.
- Any metric, threshold, user volume, or SLA-like target must either be user-provided or labeled as `proposed default`.
- If software is not the right first validation vehicle, output a `Non-Software Validation Path` using the fixed template above.
- If the information is not sufficient for a complete blueprint, output `blocking open gaps` first, then give the current draft.

## Success Standard For This Skill

The output of this skill must:

- let engineering start task breakdown instead of guessing requirements
- make v1 scope and non-scope obvious to the user
- state the riskiest hypothesis explicitly
- make the boundary between manual backstop and system capability clear
- expose every critical gap rather than hiding it with polished wording
