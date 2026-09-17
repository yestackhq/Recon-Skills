*Starting point. Edit this until it says how your team actually reviews, then publish it. Do not adopt it as-is.*

# Reviewing code

**When to use this:** Use when reviewing a pull request, a diff or a proposed change, when deciding whether something is ready to merge, or when writing code that someone else will review.

---

## Review the change that was meant, then the change that was made

Read the description and the ticket before the diff. The review is the gap between the two: what was asked for and not done, what was done and not asked for. A diff read on its own can only be judged on whether it is tidy, and tidy is the least important thing about it.

## Every comment says what happens if it is ignored

"Consider extracting this" carries no information about whether you would block on it. Write the consequence instead: "if this is not extracted, the retry logic gets duplicated in the worker next sprint", or "style preference; merge without it if you disagree". A reviewer whose comments all sound the same weight gets all of them treated as optional.

## A fix and a cleanup do not share a pull request

The fix is urgent and small; the cleanup is optional and large. Put them together and the fix waits on the cleanup's review, and when the cleanup causes a regression the fix gets reverted with it. Ask for the split, even when both halves are good.

## Ask for the test that would have caught it, not for more tests

"Add tests" produces tests for the easy cases. For a bug fix the question is specific: which test, had it existed, would have failed before this change? If the author cannot name one, the bug is not understood yet, and that matters more than coverage.

## A new abstraction needs a second caller

An interface with one implementation and a helper with one call site are guesses about the future. They are sometimes right. But the default is to inline until the second caller exists, because the second caller is what tells you the shape of the abstraction, and a guess made without it has to be undone later by someone who did not make it.

## Approve means "I would ship this"

Not "I read it", not "looks fine", not "I trust the author". If you would not be comfortable being paged for it, do not approve it; say which part you are not comfortable with. An approval that means less than that is not worth the click, and the author cannot tell the difference between it and a real one.

## Say what you did not read

A large change is rarely reviewed in full. That is acceptable; pretending otherwise is not. "I reviewed the API changes and skimmed the migration" tells the author and the next reviewer exactly where the attention went. Silence on a file reads as approval of it.

## The author's doubt is the best signal in the diff

A comment like "not sure this is right" or a commit called "try fix" points at the part that needs the most attention. Go there first. Authors flag their own weak spots more reliably than any reviewer finds them.

## What a review from us looks like

- The intended change restated in one line, and whether the diff does it
- Blocking comments separated from the rest, each with the consequence of ignoring it
- The one test that would have caught the bug, named
- What was read carefully and what was not
