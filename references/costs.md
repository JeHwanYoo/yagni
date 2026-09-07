# What a speculative part costs

The comparison in `SKILL.md` — build now versus build later — rests on this cost model. Read it when the tradeoff is not obvious.

## Four costs

A part built for a presumed future need incurs some or all of these. All four are Fowler's, from his 2015 essay.

- **Build.** The analysis, code and tests spent on something that turns out unused. Paid when you guessed wrong.
- **Delay.** The value foregone because that effort did not go to what was needed now. Fowler's insurance example: two months spent on piracy pricing that nobody bought, while storm-risk pricing that customers were waiting for shipped two months late. Paid in every case, including when the guess was right.
- **Carry.** The complexity tax the unused part levies on every change made between now and the day it is needed — or forever, if that day never comes. Unused code still has to be read, kept compiling, kept passing tests, and reasoned around. Paid in every case.
- **Repair.** The debt paid when the need was real but you built it before you understood it: the right feature, built wrong. Paid when you guessed right but early.

Build applies when you guessed wrong. Repair applies when you guessed right but early. Delay and carry apply on every branch, including the one where the guess was perfect. No branch is free.

## The comparison

People build presumptive features because they believe building now is cheaper than building later. Fowler's rule: that comparison has to be made at least against the cost of delay, and preferably weighted by the probability that the feature is unnecessary at all.

He puts that probability at two thirds or worse. The number comes from Kohavi et al. at Microsoft, who found that of features built and deployed on products, even after careful up-front analysis, only about a third improved the metric they were designed to improve. Note what that measures — shipped product features against their own success metric, not interfaces or config flags. Carry it as a floor on your doubt, not as a probability to compute with.

The thought experiment Fowler uses when mentoring: imagine the refactoring you would have to do later to add this when it is actually needed. Usually that is enough to see it will not be much more expensive later. The same exercise sometimes reveals the opposite — something cheap now, adding minimal complexity, that sharply lowers the later cost. His example is holding error messages in a lookup table instead of inline. Those moves are not YAGNI violations.

## Why your memory argues the other way

Fowler is candid that YAGNI sometimes fails: you defer, and the change is later expensive. He adds that these cases are hard to spot in advance and much easier to remember than the cases where YAGNI saved effort, and names the reason — availability bias. The time a missing abstraction hurt is vivid. The fifty times an absent abstraction was never missed left no trace. When your recollection says "we always end up needing this", discount it.

## Where YAGNI stops

**Internal quality is not tradeable.** Fowler separates external quality (the UI, defects — things a customer sees and might trade for price) from internal quality (architecture, the code). It makes sense to trade cost for external quality; it makes no sense to trade cost for internal quality, because internal quality is what lowers the cost of every future change. Poor internal quality slows the next change within weeks, so there is almost no window in which cutting it buys speed. YAGNI reduces surface; it does not reduce quality.

**YAGNI presupposes the enabling practices.** Deferring is only cheap if the code is easy to change. Fowler names the enablers — testing, continuous integration, refactoring — and says you cannot do the parts of evolutionary design that exploit a flat change curve without doing the parts that flatten it. Ron Jeffries puts the same point from the XP origin: YAGNI is using a simpler but correct solution, and it requires installing that solution with the same professionalism you would use for the elaborate one. He gives the failure its own name — skimping — and separates it from YAGNI outright.

**Passing tests outranks fewest elements.** In Fowler's rendering of Beck's rules for simple design, "passes the tests" comes first and "fewest elements" last. The ordering is Fowler's phrasing and the middle two rules are argued about, but the endpoints are not: minimality never outranks working.

## Sources

- Fowler, "Yagni" (2015) — martinfowler.com/bliki/Yagni.html. Cost model, the ⅔ figure (footnote 3), availability bias (footnote 4), the lookup-table exception, the scope limit.
- Fowler, "Is High Quality Software Worth the Cost?" — martinfowler.com/articles/is-quality-worth-cost.html. Internal versus external quality, "within a few weeks".
- Fowler, "Is Design Dead?" — martinfowler.com/articles/designDead.html. Enabling practices, refactoring is not a YAGNI violation.
- Fowler, "BeckDesignRules" — martinfowler.com/bliki/BeckDesignRules.html.
- Jeffries, "YAGNI, yes. Skimping, no. Technical Debt? Not even." — ronjeffries.com/articles/019-01ff/iter-yagni-skimp/.
- Kohavi et al., "Online Experimentation at Microsoft" (2009), the source of the one-third figure.
