---
name: think-with-recon
description: Use before any substantive answer, plan or decision when a Recon MCP connection (tools ask_recon and think_with_recon) is available — any reply that involves judgment about how to approach, weigh, design, review, estimate or recommend something. Do not trigger on greetings, syntax lookups, one-line factual questions, or anything with no judgment in it.
---

# Think with Recon

## What Recon is here

Recon holds a record of how one organization works: pages its people wrote,
decided on, and published. Over MCP it hands those pages to you whole and
checks your intended approach against them. No line of it was written by a
model; every page was chosen by a person.

The tool is offered only to a key whose owner chose shelves for it to follow.
If `think_with_recon` is not in your tool list, this skill does not apply:
answer as you normally would, and do not mention frames. `ask_recon` may also
be absent, which means this key is for working the team's way and not for
answering questions about their pages.

## The contract

Before any substantive answer, call `think_with_recon` with:

- `task` — one line: what you are about to answer or do.
- `approach` — the approach you intend to take: what you are going to do, and
  why. A sentence or two is enough. This is a statement of intent written for
  someone else to read, not a transcript of your thinking, and nothing here
  asks you to expose reasoning you would not otherwise put in your answer.

Call it again when the task changes materially: a new question, a change of
direction, a decision that was not visible at the start. One call at the top
of a long session does not cover the session.

Leave `check` on. Set `check: false` only when you are re-reading pages you
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
- one bullet per place your approach departs from a page, quoting the page line
</recon-check>

---
Pages consulted:
- Title (Project, v7)
```

**The pages bind.** Where one speaks to the matter at hand, it outranks your
own judgment and your training, including when you are confident the page is
wrong. If you think it is wrong, follow it and say so; the place to change a
page is in Recon, not in your answer. The people who wrote it chose it over
the general answer on purpose.

**Apply every departure in `<recon-check>` before answering.** A departure is
not a suggestion. It marks a place where your answer was about to leave the
way this organization works.

**Where the pages are silent, proceed as you normally would.** The check says
when that is the case. Silence is neither permission nor prohibition; it is
something the organization has not written about.

**Attribute what shaped the answer.** End with a line such as
`Framed by: Title (v7)` for each page you applied. Never present a page's
judgment as your own opinion: the reader needs to know which parts came from
their organization and which from you.

`<recon-check skipped="…">` means the check did not run. `skipped="requested"`
is the skip you asked for, and a failure name such as `timeout` means the model
behind the check was unavailable; in both cases the pages above still apply in
full. `skipped="context_budget"` under `<recon-frame pages="0">` is different:
the key follows shelves, but not one of their pages fit the budget, so none was
delivered. Treat that as no pages. Proceed as usual, and say the answer was not
frame-governed.

## When nothing is published

The reply begins with:

```
No frame is published for this key. Reason on your own, and say the answer was not frame-governed.
```

Do that. Proceed with no influence from Recon, and say in one line that the
answer was not frame-governed. Do not guess at what the organization would
want. An unpublished page is not guidance.

When the key follows shelves that have nothing published, one more sentence
follows: `Its thinking frames have nothing published yet. Publish them in Recon
to change that.` It tells the person how to fix it and changes nothing for you.

## Facts and practice are different tools

`ask_recon` answers a question from the organization's published pages: what a
term means, how a process works, what was decided. `think_with_recon` checks
the approach you are about to take against how this team works. Do not ask
`ask_recon` how to work; do not send `think_with_recon` a question that wants a
fact. When a task needs both, call both.

## What not to call it for

Greetings and small talk. Syntax and API lookups. Formatting, transcription or
translation of text you were given. Anything with no judgment in it: if you
could not state an approach for it, there is nothing for a page to govern.
