---
name: yagni
description: 'Review or simplify plans, implementations, workflows, and skills by choosing the smallest complete design for the current need. Use for explicit YAGNI, KISS, DRY, compression, keep-it-simple, or overengineering requests, and when a layer, file, interface, option, dependency, helper, extension point, delegation, or adjacent cleanup may lack a current consumer. Preserve requested scope, correctness, evidence, tests, and validation.'
metadata:
  short-description: "Smallest complete design; balance YAGNI, KISS, DRY"
---

# YAGNI

Deliver the smallest complete solution for the current need. YAGNI sets the scope; KISS and DRY refine the design that remains.

## Use three lenses

Apply these as judgment criteria, not mechanical rules. Current requirements, observed behavior, project constraints, and material risks decide the answer.

1. **YAGNI — choose what belongs.** Defer a capability built only for a presumed future when adding it later is reasonably cheap. A future-facing choice that adds no meaningful complexity now is not a YAGNI problem.
2. **KISS — choose the clearest complete form.** Among designs that meet the present need, prefer fewer concepts, branches, configuration points, and layers of indirection. Simple means easy to understand and change, not merely short or clever.
3. **DRY — keep shared knowledge authoritative.** Unify facts, rules, and invariants that must change together. Similar-looking code may remain duplicated until its shared meaning and stable shape are visible; do not create an abstraction only to remove visual repetition.

Correctness and the requested outcome come first. These principles reduce accidental surface; they do not authorize reduced scope, missing error handling, weaker tests, or skipped validation.

## Audit before simplifying

For an existing bounded system, inspect its complete component tree and references before editing. Include files, documents, wrappers, generated artifacts, layers, and execution steps—not only the prose or code named in the request.

For each component ask separately:

1. What present behavior or consumer needs its content?
2. Why must that content exist in this separate component?
3. Is another component already authoritative for the same knowledge or behavior?

Keep required behavior; remove an unreferenced component or fold it into the authority that already enforces it. Re-run the inventory after editing and verify the remaining execution path.

## Decide whether to defer

Consider three questions:

1. **Does it add complexity now?** Name the capability, concept, branch, configuration, or coordination cost it introduces.
2. **Is it justified now?** Look for a requirement, observed failure, project constraint, improvement to current clarity, or material risk. Tests, continuous integration, and refactoring that keep the current change safe are enabling work, not optional polish. Refactoring justified only by a hypothetical future remains subject to YAGNI.
3. **Is later actually cheaper?** Picture the concrete edits required when the need arrives. Deferral is unattractive when the change would touch every call site, the code cannot be changed safely, or the shape is already known from present cases.

Use the cost model in `references/costs.md` when the comparison is not obvious.

## Make non-obvious decisions inspectable

Do not produce a record for every helper or restate obvious choices. When a speculative part materially affects the design, or the requester asks for a YAGNI review, use this compact comparison if it helps expose the evidence:

```text
<part>
  needed now by: <requirement, observed failure, project constraint,
                  current clarity, or material risk — and where it appears>
  if deferred, later cost: <specific edits required later>
  decision: <keep, simplify, or defer — with the deciding tradeoff>
```

A part with no present need and a cheap later edit normally waits. This is a default, not an automatic deletion rule. A cheap seam with a costly retrofit may stay when its shape is known; an abstraction based only on a guessed second case normally waits. Anything kept is built only for the present cases.

## Read the relevant reference

| The part is | Read |
|---|---|
| an interface, registry, plugin seam, option or flag for a second thing that is planned, not present | `references/abstraction.md` |
| similar code you may unify, duplicated knowledge, or an abstraction that no longer fits | `references/duplication.md` |
| something that would make the current result wrong if omitted — authorization, validation, rollback, idempotency, or a retrofit-hostile seam | `references/correctness.md` |
| a delegated agent, parallel orchestration, a new skill or automation, or an intermediate document | `references/beyond-code.md` |

## Boundaries

The smallest complete solution may be a broad change. Make it broad when the present requirement demands it. Do not narrow the requested outcome, trade away internal quality, or confuse fewest lines with simplest design.

Before finishing, check both directions: every material part kept has a present justification or a convincing retrofit cost, and every requested behavior still has a concrete implementation and verification path. Explain only the decisions that affect scope, risk, or a reviewer’s ability to overrule the judgment. State any validation you could not perform.
