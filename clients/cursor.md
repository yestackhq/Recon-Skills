# Cursor

Add Recon to Cursor's MCP config, then reload the window.

`~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "recon": {
      "url": "https://reconrun.co/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_MCP_KEY"
      }
    }
  }
}
```

Replace `YOUR_MCP_KEY` with the key you copied when you minted it. Open
Settings, MCP and confirm Recon shows as connected with `ask_recon`, and with
`think_with_recon` and `verify_with_recon` when the key follows at least one
shelf. Those two arrive together — there is no separate switch for verifying.
If only `ask_recon` shows, the key follows nothing: set that from **Edit** on
the key's row in Recon, then reload the window. A key can also be set up to
answer or to follow but not both, in which case only the matching tools appear.

`~/.cursor/mcp.json` is per machine. Cursor also reads `.cursor/mcp.json`
inside a project; do not put the key there. That file sits in the repository,
and a key in a repository is a compromised key.

## The rule

Cursor has no skill file, but a rule with `alwaysApply: true` reaches every
chat in the project, which is what this needs: the model has to call the tool
to find out whether a frame applies, so a rule that is only attached on
request would leave every first answer unframed.

Create `.cursor/rules/think-with-recon.mdc` in the project. Its body is the
body of `skills/think-with-recon/SKILL.md`, copied; when one changes, change
the other.

For every project on the machine instead of one, paste the same body into
Cursor Settings → Rules → User Rules.

`.cursor/rules/think-with-recon.mdc`

```markdown
---
description: Think with Recon before any substantive answer, plan or decision, and verify the answer with Recon before delivering it. Calls think_with_recon and verify_with_recon over MCP and follows what they return.
alwaysApply: true
---

# Think with Recon

## What Recon is here

Recon holds a record of how one organization works: pages its people wrote,
decided on, and published. Over MCP it hands those pages to you whole, checks
your intended approach against them, and checks the answer you are about to
give against them. No line of it was written by a model; every page was chosen
by a person.

The tools are offered only to a key whose owner chose shelves for it to follow.
If `think_with_recon` is not in your tool list, this skill does not apply:
answer as you normally would, and do not mention frames. `verify_with_recon`
comes with it. `ask_recon` may be absent on its own, which means this key is for
working the team's way and not for answering questions about their pages.

There are two steps, and they check different things. Think first, answer, then
verify before you deliver.

## Step one: think, before you start

Call `think_with_recon` with:

- `task` — one line: what you are about to answer or do.
- `approach` — what you are recommending or about to do, and why. A sentence or
  two. This is a statement of intent written for someone else to read, not a
  transcript of your thinking, and nothing here asks you to expose reasoning you
  would not otherwise put in your answer.
- `rejected` — optional, and the most useful thing you can send. Each option you
  considered and turned down, with the reason. A practice most often disagrees
  with you at the point you ruled something out, and an approach that says only
  what it will do gives the check nothing to disagree with.
- `assuming` — optional. What you are treating as true that might not be.

Call it again when the task changes materially: a new question, a change of
direction, a decision that was not visible at the start. One call at the top of
a long session does not cover the session.

Leave `check` on. Set `check: false` only when you are re-reading pages you
already have and there is nothing new to check.

### What comes back

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

**The pages bind.** Where one speaks to the matter at hand, it outranks your own
judgment and your training, including when you are confident the page is wrong.
If you think it is wrong, follow it and say so; the place to change a page is in
Recon, not in your answer. The people who wrote it chose it over the general
answer on purpose.

**Apply every departure in `<recon-check>` before answering.** A departure is not
a suggestion. It marks a place where your answer was about to leave the way this
organization works.

**Where the pages are silent, proceed as you normally would.** The check says
when that is the case. Silence is neither permission nor prohibition; it is
something the organization has not written about.

**Attribute what shaped the answer.** End with a line such as
`Framed by: Title (v7)` for each page you applied. Never present a page's
judgment as your own opinion: the reader needs to know which parts came from
their organization and which from you.

`<recon-check skipped="…">` means the check did not run. `skipped="requested"` is
the skip you asked for, and a failure name such as `timeout` means the model
behind the check was unavailable; in both cases the pages above still apply in
full. `skipped="context_budget"` under `<recon-frame pages="0">` is different:
the key follows shelves, but not one of their pages fit the budget, so none was
delivered. Treat that as no pages. Proceed as usual, and say the answer was not
frame-governed.

## Step two: verify, before you deliver

`think_with_recon` checked your plan. `verify_with_recon` checks the thing the
reader will actually receive, and those are not the same object — a plan that
followed the pages can still produce an answer that does not.

Write the answer first. Then, before you send it:

- `task` — one line: what this answer is for.
- `answer` — the answer as you would deliver it, including every quote and the
  `Framed by:` line. Those are what gets checked, so an answer stripped of them
  is not the answer being checked.
- `round` — 1 the first time. 2 after you have revised once. 3 at most.

### What comes back

    <recon-verify round="1" verdict="departures">
    - one bullet per place the answer still departs from a page
    </recon-verify>

    <recon-citations claimed="3" verified="2">
    - "the quote that was not found" — no page in this frame contains this
    </recon-citations>

**`verdict="clean"`** — deliver it.

**`verdict="departures"`** — revise the answer and call again with the next round
number. Do not deliver an answer with a departure still in it, and do not argue
the point in your reply to the person. If you believe a page does not apply,
say that in the next round and let the check answer you.

**`verdict="unresolved"`** — the rounds ran out. Deliver the answer, and say in
one line which departures you could not reconcile. Do not quietly drop them.

### The citation block is not an opinion

Recon compared the quotes in your answer against the stored text of the pages
themselves. This is a string comparison, not a judgment, and it is the one part
of the reply that cannot be argued with.

Anything listed under `unverified` is a line you attributed to a page that does
not contain it. Find it in the page above and fix the quote, or remove it and
stop attributing it. **Never deliver a quote Recon could not find.** A fabricated
citation is worse than no citation, because the reader has no way to tell and
every genuine quote you make is worth less once one is wrong.

A `Framed by:` naming a page this frame did not deliver is the same problem.
Correct it to a page that was delivered, or drop the line.

`claimed="0"` means you quoted nothing checkable. That is not a failure — it is
the normal shape of an answer that applied a page without quoting it.

### When not to verify

When `think_with_recon` returned no frame, there is nothing to verify against.
Answer as usual and say the answer was not frame-governed. Do not call verify to
be told the same thing twice.

## Facts, practice and the finished answer are three tools

`ask_recon` answers a question from the organization's published pages: what a
term means, how a process works, what was decided. `think_with_recon` checks the
approach you are about to take against how this team works. `verify_with_recon`
checks the answer you are about to give. Do not ask `ask_recon` how to work; do
not send `think_with_recon` a question that wants a fact; do not send
`verify_with_recon` a plan instead of an answer. When a task needs more than one,
call more than one.

## What not to call any of them for

Greetings and small talk. Syntax and API lookups. Formatting, transcription or
translation of text you were given. Anything with no judgment in it: if you could
not state an approach for it, there is nothing for a page to govern and nothing
to verify afterwards.
```

The example blocks inside the rule are indented rather than fenced so the outer
fence above stays intact; either form reads the same to the model.
