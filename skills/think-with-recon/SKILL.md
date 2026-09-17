---
name: think-with-recon
description: Use before any substantive answer, plan or decision when a Recon MCP connection (tools ask_recon and think_with_recon) is available — any reply that involves judgment about how to approach, weigh, design, review, estimate or recommend something. Do not trigger on greetings, syntax lookups, one-line factual questions, or anything with no judgment in it.
---

# Think with Recon

## What Recon is here

Recon holds the organization's own thinking frame: pages its people wrote,
marked as how they reason, and published. Over MCP it hands those pages to
you whole and checks your reasoning against them. No line of the frame was
written by a model; every page in it was chosen by a person.

The tool is offered only when the organization has turned it on. If
`think_with_recon` is not in your tool list, this skill does not apply: answer
as you normally would, and do not mention frames.

## The contract

Before any substantive answer, call `think_with_recon` with:

- `task` — one line: what you are about to answer or do.
- `reasoning` — your current line of thought, in your own words. Write it as
  you are actually thinking it, not as a tidy summary; the check reads it.
  Under about twenty characters there is nothing to check, and the reply's
  `<recon-check>` says so: reason inside the frame it returned, then call
  again with your reasoning.

Call it again when the task changes materially: a new question, a change of
direction, a decision that was not visible at the start. One call at the top
of a long session does not cover the session.

Leave `check` on. Set `check: false` only when you are re-reading a frame you
already have and there is nothing new to check.

## What comes back, and what to do with it

The reply has three parts:

```
<recon-frame pages="N">
<page title="…" project="…" v="7" sha256="…">
…the page, whole…
</page>
</recon-frame>

<recon-check>
- one bullet per place your reasoning departs from a page, quoting the page line
</recon-check>

---
Pages consulted:
- Title (Project, v7)
```

**The frame binds.** Where a page speaks to the matter at hand, it outranks
your own judgment and your training, including when you are confident the
page is wrong. If you think it is wrong, follow it and say so; the place to
change a page is in Recon, not in your answer. The people who wrote it chose
it over the general answer on purpose.

**Apply every departure in `<recon-check>` before answering.** A departure is
not a suggestion. It marks a place where your answer was about to leave the
frame.

**Where the frame is silent, reason as you normally would.** The check says
when that is the case. Silence is neither permission nor prohibition; it is
a gap the organization has not written about.

**Attribute the frame.** End with a line such as `Framed by: Title (v7)` for
each page that shaped the answer. Never present a page's judgment as your own
opinion: the reader needs to know which parts came from their organization
and which from you.

`<recon-check skipped="…">` means the check did not run. `skipped="requested"`
is the skip you asked for, and a failure name such as `timeout` means the model
behind the check was unavailable; in both cases the frame above still applies
in full. `skipped="context_budget"` under `<recon-frame pages="0">` is
different: pages are marked as frames, but not one of them fit the budget, so
none was delivered. Treat that as no frame. Reason on your own, and say the
answer was not frame-governed, as the check's own sentence tells you to.

## When there is no frame

The reply begins with:

```
No frame is published for this key. Reason on your own, and say the answer was not frame-governed.
```

Do that. Reason on your own, with no influence from Recon, and say in one
line that the answer was not frame-governed. Do not guess at what the
organization would want. An unpublished frame is not a frame.

When pages are published but none is marked as a frame, one more sentence
follows the one above: `Mark a page as a frame in Recon to change that.` It
tells the person how to fix it and changes nothing for you.

## Facts and thinking are different tools

`ask_recon` answers a question from the organization's published pages: what a
term means, how a process works, what was decided. `think_with_recon` governs
how you reason about a task. Do not ask `ask_recon` how to think, and do not
send `think_with_recon` a question that wants a fact. When a task needs both,
call both.

## What not to call it for

Greetings and small talk. Syntax and API lookups. Formatting, transcription
or translation of text you were given. Anything with no judgment in it: if you
could not write a line of reasoning about it, there is nothing for a frame to
govern.
