---
name: think-with-recon
description: Use before a substantive answer, plan, recommendation or decision when think_with_recon and verify_with_recon are available. Skip greetings, simple syntax lookups and tasks with no judgment.
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

- `task`: what the user wants, in one line.
- `approach`: the concrete approach you intend to take and why. Do not send a
  placeholder or private reasoning transcript.
- `rejected`: optional alternatives you ruled out, with reasons.
- `assuming`: optional assumptions that may affect the decision.

Read Recon's comparison. If it names a departure, change the approach as it
directs, then continue. A page speaks for its author; attribute any team
practice you apply by title and version. Recon may say the pages do not cover
the matter. In that case use your ordinary judgment and do not invent a team
position. If no frame is published, say the answer was not frame-governed.

A failed or skipped check is not alignment. Retry `think_with_recon` when it
asks for a fuller approach or reports a temporary failure. If Recon reports
that pages were left out because of the context budget, do not claim it checked
the complete frame.

Call `think_with_recon` again when the task or your approach changes materially.
One call at the start does not cover a new decision later in the conversation.

## Before replying: send the exact draft

Draft the answer you intend to give the user, including all quotes and a
`Framed by: Title (v7)` line for each page whose practice shaped it. Before
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

- `clean`: deliver the checked draft. If you edit it, verify the edited version.
- `departures`: revise each point Recon names, then send the revised *whole*
  draft back with the next round number and that reply as `previous`. Do this
  before replying to the user.

  If a finding is about something the user explicitly asked for, you may keep
  it, because that is the user's call. Say so in the draft instead: name what
  the page requires and that the answer departs from it at the user's request.
  Then send that draft to the next round like any other. Deciding not to change
  something is not a reason to stop checking.
- `unresolved`: the check failed or reached its three-round limit. If the
  reply's `Next:` line says to retry, retry. Otherwise tell the user which
  findings remain unresolved, and do not present the answer as verified.

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
say which findings remain.

Until a check returns clean, do not tell the user the draft was checked,
verified or approved. Say what Recon found, or that the check is still going.
