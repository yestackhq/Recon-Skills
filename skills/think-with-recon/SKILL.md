---
name: think-with-recon
description: Use before any plan, recommendation, decision or other substantive answer when think_with_recon and verify_with_recon are available, including a plan the user dictated point by point, since whether it fits how the team works is what Recon checks. Skip greetings, small talk and simple lookups.
---

# Think with Recon

Recon holds pages that people in this organization chose and published as the
way their team works. Recon reads those pages inside its own service. Its MCP
reply gives you a comparison and page attribution, not the page bodies.

This skill applies only when `think_with_recon` and `verify_with_recon` are in
your tool list. `ask_recon` may be available separately for factual questions
about the organization's pages.

## Before working: send your approach

Call `think_with_recon` before a substantive answer or decision. Send:

- `task`: what the user wants, in one line, with every explicit instruction
  they gave: dates, limits, exclusions, format. Recon checks the approach and
  the answer against these as well as against the pages.
- `approach`: the concrete approach you intend to take and why. Do not send a
  placeholder or private reasoning transcript.
- `rejected`: optional alternatives you ruled out, with reasons.
- `assuming`: optional assumptions that may affect the decision.

Read Recon's comparison. If it names a departure, change the approach as it
directs, then continue. Where the user ruled something out, look first for the
way to meet the page that stays inside their limits; Recon often suggests one.
A page speaks for its author; attribute any team practice you apply by title.
The version belongs in the `Framed by:` line only, not in sentences the user
reads, where it looks like a detail of the page. Recon may say the pages do not
cover the matter. In that case use your ordinary judgment and do not invent a
team position. If no frame is published, say the answer was not frame-governed.

A failed or skipped check is not alignment. Retry `think_with_recon` when it
asks for a fuller approach or reports a temporary failure. If Recon reports
that pages were left out because of the context budget, do not claim it checked
the complete frame.

Call `think_with_recon` again when the task or your approach changes materially.
One call at the start does not cover a new decision later in the conversation.

## Then answer what the user asked

Recon's replies are for you. Once your approach is checked, answer the user's
original request with the findings applied: the plan, note, code or decision
they asked for. The frame shapes how you answer; it is not the answer. Do not
hand the user the frame or Recon's findings instead, and do not paste Recon's
reply unless they asked to see it. Name a page in the answer only where it
changed, or conflicts with, something the user asked for, and put the rest in
the `Framed by:` line.

Keep what the user explicitly asked for: dates, limits, exclusions, format. If
one of their choices looks risky, keep it, say what could go wrong and what
would justify changing it, and let them decide. Change it yourself only where a
page requires it, as described under `departures` below.

When you say what a page asks for, use its own terms. Do not narrow an ongoing
practice into a single step, or add a requirement it does not state; the user
may check the page, and a paraphrase that says more or less than it does is
wrong even when it is not a quote.

## Before replying: send the exact draft

Draft the answer you intend to give the user, including all quotes and a
`Framed by: Title (v7)` line for each page whose practice shaped it. Write it
in its final wording, with any style or formatting rules you follow already
applied: the text you verify is the text you send. Rewording it after the
check changes the answer, and the changed answer has not been checked. Before
you deliver that draft, call `verify_with_recon` with:

- `task`: what this answer is for.
- `answer`: the exact draft, with quotes and attribution intact.
- `round`: `1` on the first pass.
- `previous`: the last Recon reply, pasted as it came back — the
  `think_with_recon` reply on round 1, the last `verify_with_recon` reply after
  that. Each check is independent and remembers nothing; this is how it knows
  what was already found, so a finding that still stands is not forgotten
  between rounds.

Treat the returned verdict as a gate for this draft:

- `clean`: deliver the checked draft word for word. If you change anything,
  even the wording, verify the changed version first.
- `departures`: revise each point Recon names, then send the revised *whole*
  draft back with the next round number and that reply as `previous`. Do this
  before replying to the user.

  If a finding is about something the user explicitly asked for, first look for
  a way to meet the page without it, such as a step that does what the page
  asks without the thing they ruled out. If there is one, add it. If there is
  none, keep the user's choice, because that is their call, and say so in the
  draft: what the page requires, in its own terms; what it says happens without
  it; and that the answer departs from it at the user's request. Then send that
  draft to the next round like any other. Deciding not to change something is
  not a reason to stop checking.
- `unresolved`: the check failed or reached its three-round limit. If the
  reply's `Next:` line says to retry, retry. Otherwise deliver the draft you
  sent in that round word for word, adding only a short note of which findings
  remain unresolved. Do not reword the rest: those are the last words Recon
  checked. Do not present the answer as verified.

The citation report is checked against published text. If Recon says a quote
was not found, fix or remove it and verify the revised draft again. If a
`Framed by:` line names an unknown page, correct or remove that line. A clean
model judgment cannot override a citation failure.

Do not substitute `ask_recon` for either check. It answers factual questions;
`think_with_recon` compares your approach; `verify_with_recon` compares the
answer the user would actually receive.

## The final check

Before any substantive reply, ask yourself: **Is this exact draft the one I
sent to `verify_with_recon`, and did it return clean?** If the answer is no,
call `verify_with_recon` now. Deliver a draft without a clean check only when
Recon's last reply on it was `unresolved` and did not ask for a retry, and then
deliver that draft word for word with a short note of which findings remain.

Until a check returns clean, do not tell the user the draft was checked,
verified or approved. Say what Recon found, or that the check is still going.
