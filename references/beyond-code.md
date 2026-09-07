# The part is not code

Agents delegate work, orchestrate parallel runs, create skills and automation, and produce documents about the work. Each of these has the same four costs as an interface — build, delay, carry, repair — paid in time, tokens, context and coordination failure instead of lines. Use the same comparison when the choice is material.

## Delegated agents and parallel orchestration

A delegate starts with none of what you know. Briefing it costs tokens; its result comes back without the judgment you would have applied while doing the work; and if two delegates touch the same files, reconciling them is a cost you did not have when there was one of you.

Present justifications include:

- the work will not fit in one context — reading forty files, sweeping a codebase, comparing many sources
- the work needs isolation you actually want — an independent opinion, a check that must not share your assumptions, a worktree that must not disturb yours

Not a justification: the task *could* be parallelized. Most tasks could. The question is what the parallelism buys against the briefing, the reconciliation and the judgment that does not travel.

Verification passes are the common speculative case. A second agent re-reading your output through the same lens is carry with a label on it. A verifier is warranted when it brings evidence you did not have — a different lens, a test you did not run, a source you did not read.

## Skills, standing automation and configuration

A skill, a trigger that fires on an event, a scheduled job, a config entry: each one is code that runs on every future occasion whether or not the occasion needs it. Its present justification is a recurring need with more than one instance behind it.

A procedure you used once is a note, not a skill. Writing it up as one is the interface with a single implementation — you have abstracted from one case, and the second case will not match.

## Intermediate documents

Plans, summaries, reports, status write-ups, this-is-what-I-did files. The comparison is simple: who downstream reads this, and what do they do with it. A plan the requester will follow has a justification. A plan nobody will open, produced because producing plans feels like diligence, costs the reader's attention on the way to the thing they asked for.

Separate the need for the information from the need for another artifact. A format described in a document may be required while the document is not: if a renderer already enforces that format, the renderer is the authority and the duplicate template schedules drift. Keep a separate document only when a current consumer reads it for a purpose the executable authority does not serve.

An explanation of a material scope decision has a reader: the reviewer who may overrule it. Include the evidence and the limit of what you verified; do not create a separate report when a sentence in the final reply is enough.

## The same test, restated

For each: who needs it now, and what does it cost to add later. An agent you did not spawn can be spawned in one turn. A skill you did not write can be written when the second occasion arrives. A report you did not produce can be produced when someone asks. The later cost is almost always one action, and the carry of the unneeded one is paid on every turn in between.
