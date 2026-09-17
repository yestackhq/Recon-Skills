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
`think_with_recon` when the key follows at least one shelf. If only
`ask_recon` shows, the key follows nothing — set that from **Edit** on the
key's row in Recon, then reload the window. A key can also be set up to do only
one of the two, in which case only that tool appears.

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
description: Think with Recon before any substantive answer, plan or decision. Calls think_with_recon over MCP and follows the frame it returns.
alwaysApply: true
---

# Think with Recon

## What Recon is here

Recon holds the organization's own thinking frame: pages its people wrote,
marked as how they reason, and published. Over MCP it hands those pages to
you whole and checks your intended approach against them. No line of the frame was
written by a model; every page in it was chosen by a person.

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

Leave `check` on. Set `check: false` only when you are re-reading a frame you
already have and there is nothing new to check.

## What comes back, and what to do with it

The reply has three parts:

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
different: the key names frame shelves, but not one of their pages fit the
budget, so none was delivered. Treat that as no frame. Reason on your own, and say the
answer was not frame-governed, as the check's own sentence tells you to.

## When there is no frame

The reply begins with:

    No frame is published for this key. Reason on your own, and say the answer was not frame-governed.

Do that. Reason on your own, with no influence from Recon, and say in one
line that the answer was not frame-governed. Do not guess at what the
organization would want. An unpublished frame is not a frame.

When pages are published but none is marked as a frame, one more sentence
follows the one above: `Its thinking frames have nothing published yet. Publish them in Recon to change that.` It
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
could not state an approach for it, there is nothing for a page to
govern.
```

The two example blocks inside the rule are indented rather than fenced so the
outer fence above stays intact; either form reads the same to the model.
