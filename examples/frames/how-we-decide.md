*Starting point. Edit this until it says what your organization actually believes, then publish it. Do not adopt it as-is.*

# How we decide

**When to use this:** Use when the work is a decision — choosing between options, recommending a course of action, deciding whether to do something at all, or answering "what should we do about X".

---

## Say who decides before saying what

Most of our bad decisions were not wrong; they were unowned. Made in a thread, agreed by nobody in particular, reversed by whoever noticed next. Before weighing options, name the person who decides and the people who need to know. If you cannot name the decider, that is the first problem, and the recommendation is "find one".

## Sort by how hard it is to undo

A choice that can be reversed in a day gets made by whoever is closest to it, today, with the information at hand. A choice that cannot be reversed — a contract, a public API, a migration that drops data, a hire — gets written down first: the options, the reason, the observation that would prove it wrong. We spend our deliberation on the second kind. Treating every choice as the second kind is how nothing ships; treating every choice as the first is how we ended up with three billing systems.

## Write the reason, not just the outcome

"We chose Postgres" is useless in a year. "We chose Postgres because the team knows it and the workload is small; revisit if writes pass 5k/s" tells the next person when the decision expired. A recommendation without the condition under which it would change is an opinion, not a decision.

## Silence is not agreement

A proposal nobody objected to in a channel has not been agreed. It has been not-read. Agreement is a named person saying yes. When a recommendation depends on someone else's buy-in, list it as a step, not as an assumption.

## Prefer the option that keeps the next decision cheap

When two options are close, take the one that leaves more open. Not because optionality is free — it is not — but because we are usually deciding with less information than we will have in a month, and the option that does not foreclose the alternatives is the one that lets that month's information count.

## Do not re-open a decision without new information

Relitigating a settled decision because it feels uncomfortable is a tax on everyone who did the work the first time. Re-open it when something has changed: a number, a constraint, a failure. Say what changed in the first line.

## Cost the "do nothing" option honestly

Doing nothing is a decision with its own cost, and it is the option most often left off the list because nobody has to defend it. Put it on the list. Sometimes it wins, and it should win on merit rather than by default.

## What a recommendation from us looks like

- The decision in one sentence, and who owns it
- Whether it is reversible, and what reversing it would cost
- The options considered, including doing nothing
- The reason, and the observation that would change it
- What needs to be true that has not been checked yet
