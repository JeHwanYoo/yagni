# The part is an extraction of two similar pieces of code, or an abstraction that no longer fits

## Duplication is far cheaper than the wrong abstraction

Sandi Metz's phrasing, and the whole case in one line. Her account of how the wrong abstraction comes to exist:

1. Programmer A sees duplication, extracts it, names it. Everyone feels good.
2. A new requirement arrives that the abstraction almost fits.
3. Programmer B, honour-bound to keep the existing abstraction, adds a parameter and a conditional for the new case.
4. Repeat. The abstraction now has more branches than callers, and every caller uses a different subset.

By step four the abstraction costs more to understand than the copies it replaced, and nobody remembers which caller needs which branch. The extraction was cheap; the carry was not.

## Extract on the third occurrence

The Rule of Three, from *Refactoring*: the first time you do something, just do it. The second time you do something similar, wince, but duplicate it anyway. The third time, refactor.

The reason three and not two: two cases rarely show which part is common and which is incidental to each. With three you can usually see the shape, and you can be fairly confident there will be a fourth. An abstraction extracted at two is a guess about which of two differences is the real one.

Kent C. Dodds' name for the same instinct: AHA — Avoid Hasty Abstractions. His added rule is optimize for change first, on the grounds that you do not know what the code's future is, only that it will change, and duplicated code is easier to change in one place than a shared abstraction is to change for one caller.

## One carve-out

Two copies of the same *knowledge* are already one thing. A VAT rate, a date format, a validation invariant — anything that is obliged to change in both places at once. Holding those apart does not defer an abstraction; it schedules a divergence, and the divergence is a bug. Unify those at two.

The test is not "does this look the same" but "must these change together". Shared shape is not shared meaning.

## Extract only when it makes the present code clearer or more correct

DRY as a principle says nothing about when. Applied as "never write anything twice", it produces the wrong abstraction by step three of Metz's sequence. Applied as "extract when the extraction is itself an improvement to today's code", it is the same test as the rest of this skill: the extraction needs a present justification, and "these two look alike" is not one.

## When the abstraction has already gone wrong

Metz's remedy, which goes all the way back on purpose:

1. Inline the abstracted code back into **every** caller.
2. In each caller, keep only the branch that caller actually uses. Delete the parameter and the conditional it fed.
3. Now look at the callers. If a new abstraction is visible, extract it from what you can see — not from what you remember the old one was for.

Inlining at just the call site that broke leaves the wrong abstraction standing for every other caller, which is the state that produced the spiral. Going backwards deliberately is the cheaper direction.

## How DRY bounds YAGNI's blast radius

DRY keeps a deferred change local when the current code has one source of truth for the knowledge or invariant involved. It does not require two pieces of merely similar code to share an abstraction. Keep knowledge that must change together in one place; allow repeated shape until present cases reveal a stable common form. YAGNI keeps speculative surface small, and this narrower form of DRY prevents one decision from drifting across several locations without forcing an early abstraction.

## Sources

- Metz, "The Wrong Abstraction" (2016) — sandimetz.com/blog/2016/1/20/the-wrong-abstraction.
- Fowler (with Beck), *Refactoring*, the Rule of Three (attributed to Don Roberts).
- Dodds, "AHA Programming" — kentcdodds.com/blog/aha-programming. The acronym came to him from Cher Scarlett.
- Jeffries, "YAGNI, yes. Skimping, no." — ronjeffries.com/articles/019-01ff/iter-yagni-skimp/.
