---
name: blueprint-challenge
description: Adversarial review of MVP blueprints, requirement documents, PRDs, feature specs, and early product plans before implementation. Use when Codex needs to challenge a requirements artifact from an independent angle, surface logic gaps, identify riskiest assumptions, test closed-loop completeness, cut scope, run a premortem, or decide go/revise/stop before engineering begins. Also use after requirement excavation to stress-test whether the blueprint is buildable, valuable, and measurable.
---

# Blueprint Challenge

Challenge a blueprint before engineering commits to it.

The goal is not to rewrite the document. The goal is to expose whether the proposed MVP should move forward as written, be revised, or be stopped.

## Core Goal

Reach a defensible `go`, `revise`, or `stop` decision by testing the blueprint from six hostile review angles:

1. value
2. riskiest assumption
3. closed loop
4. boundary
5. premortem
6. validation

## Hard Rules

- Read the source artifact first. If the request refers to local docs, tickets, PRDs, prior blueprints, or workflow notes, inspect them before asking questions.
- Review the smallest claimed MVP loop, not the roadmap around it.
- Prioritize logic failures, missing data paths, false automation claims, and invalid success metrics over document polish.
- Do not restate the blueprint unless a compressed summary is needed to anchor the critique.
- Separate `artifact fact`, `inference`, and `recommended correction`.
- If the best fix is to shrink the MVP, say so directly.
- If software is not the right first validation vehicle, say so directly.
- Ask at most 2 questions in a round, and only when a missing fact blocks the verdict.
- If the artifact contains multiple independent products, audiences, or loops, require decomposition before issuing a strong `go`.
- Do not invent unavailable data, downstream labels, integrations, or operational capabilities.
- Do not accept heuristic output phrased as deterministic truth. Force the artifact to label heuristics honestly.

## Default Workflow

1. Read the artifact and identify the claimed first user, trigger, pain point, minimum loop, and stated success signal.
2. Name the biggest review risk immediately. Do not hide it behind a soft summary.
3. Run the six review lenses in fixed order.
4. Stop early only if a fatal flaw already invalidates the blueprint. If stopping early, still note the downstream consequences briefly.
5. Produce a verdict and the minimum corrections needed to move forward.

## Review Lenses

### 1. Value Review

Test whether the blueprint describes a real blocking problem instead of feature excitement.

Check:

- who the first user is
- when the need is triggered
- what concrete pain appears at that moment
- what the current workaround is
- why the workaround is not good enough

Call out directly:

- vague audiences such as "everyone" or "creators"
- triggers that are occasional inspiration instead of repeatable workflow moments
- pain that is convenience-only rather than blocking
- solutions that sound useful but do not change user behavior

### 2. Riskiest Assumption Review

Identify the top 1 to 3 assumptions that could invalidate the MVP if false.

Prefer assumptions about:

- data availability
- output usefulness
- cost viability
- human behavior change
- operational load
- quality thresholds hidden inside vague wording

Force the review to answer:

- what is the single most dangerous assumption
- whether v1 actually tests it
- whether the current scope hides it behind easier implementation work

### 3. Closed-Loop Review

Map the blueprint into:

- trigger
- input
- decision
- action
- state
- output
- failure branch

Call out directly when:

- a decision has no input
- an action has no owner
- a state transition is undefined
- an output is interesting but not actionable
- a failure path is ignored
- a supposedly automated step actually depends on human judgment

If a critical step depends on human judgment, require it to be labeled as `manual backstop`.

### 4. Boundary Review

Challenge whether the MVP is sliced tightly enough.

Force the artifact into four buckets:

- `MVP must-have`
- `manual backstop`
- `later validation`
- `out of scope`

Call out directly:

- stacked products pretending to be one MVP
- optional improvements disguised as must-have
- automation added before the core loop proves value
- personalization, prediction, or scale features pulled into v1 too early

### 5. Premortem Review

Assume the MVP failed after first implementation or first live trial. List the most likely reasons.

Prefer failure causes such as:

- no stable source data
- cost or ops load exceeds saved effort
- output quality looks smart but is not usable
- too much manual cleanup remains
- candidate outputs collapse into generic repetition
- the user still falls back to the old workflow

Use this section to expose what the blueprint is optimistic about but has not defended.

### 6. Validation Review

Test whether the stated success signals actually validate the product hypothesis.

Separate:

- system success: the workflow runs and produces output
- product success: the output changes user behavior and proves value

Reject metrics that only prove the system executed, for example:

- report generated
- model returned text
- pipeline completed

Prefer signals such as:

- the user actually used the output
- the output reduced a previously costly step
- the user repeated the workflow voluntarily
- the result justified continued investment

Any threshold not supplied by the user must be labeled `proposed default`.

## Common Failure Patterns

Call these out immediately when present:

- there is no recurring pain, only vague ambition
- multiple products are bundled into one MVP
- the promised output cannot be derived from the stated inputs
- human judgment is hidden inside a fake automation story
- the product is really a service workflow, not a software capability
- the stated metric measures generation, not usefulness
- the operating cost is likely higher than the value of the shortcut
- the account pool, user segment, or source corpus is too broad to create specific output
- the artifact claims prediction where only heuristics are possible

## Question Style

Ask sharp blocking questions only when the artifact leaves the verdict underdetermined.

Prefer:

- "What exact data supports this judgment?"
- "Which step is still human-only in v1?"
- "What user action proves this output is actually useful?"
- "Which feature here can be removed without breaking the loop?"

Avoid:

- broad brainstorming
- solution ideation unrelated to the review defect
- open-ended "what do you want" questions

## Output Format

Use this structure unless the user asks for a different one:

```md
# Blueprint Review

## Verdict
- Decision: `go` | `revise` | `stop`
- Core reason:
- Most dangerous unresolved assumption:

## Critical Issues
1. [critical|high|medium] <issue title>
   - Lens:
   - Problem:
   - Why it matters:
   - Evidence from artifact:
   - Required correction:

## Scope Corrections
### Keep in MVP
- ...

### Move to manual backstop
- ...

### Move to later validation
- ...

### Remove from this phase
- ...

## Lens Notes
### Value Review
- ...

### Riskiest Assumption Review
- ...

### Closed-Loop Review
- ...

### Boundary Review
- ...

### Premortem Review
- ...

### Validation Review
- ...

## Assumptions and Open Questions
- To confirm:
- Default assumptions:
- Blocking gaps:

## Recommended Next Move
- If `go`:
- If `revise`:
- If `stop`:
```

## Decision Heuristics

Use these defaults:

- `go`: the core loop is coherent, required data likely exists, major manual backstops are explicit, and success signals validate the hypothesis.
- `revise`: the blueprint is salvageable, but one or more critical issues would force engineering to guess or build the wrong thing.
- `stop`: there is no real pain, no credible data path, no executable logic between input and output, or no meaningful way to validate value.

## Success Standard For This Skill

The output of this skill must:

- surface the most dangerous flaw early
- distinguish fatal problems from polish issues
- make scope cuts obvious
- separate heuristic claims from measurable truths
- tell the user whether to `go`, `revise`, or `stop`
- give engineering and product a smaller, safer next step instead of a vague critique
