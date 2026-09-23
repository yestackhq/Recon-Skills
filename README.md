# Recon Skills

Client-side skills and starter frames for Recon's MCP server.

Recon's MCP endpoint, `https://reconrun.co/mcp`, exposes up to three tools to any
client that holds an MCP key (`rmcp_live_…`). `think_with_recon` and
`verify_with_recon` are listed only for keys that follow one or more shelves,
chosen when the key is minted; a key that follows none gets exactly the server
that existed before they did, and nothing in this repository applies to it.
`ask_recon` can be turned off per key too, so an assistant can be given
answering, framing, or both:

| Tool | You send | You get back |
|---|---|---|
| `ask_recon` | a question | an answer from the organization's published pages, citing them |
| `think_with_recon` | the task and the approach you intend to take | a concise comparison with concrete corrections and page attribution |
| `verify_with_recon` | the task and the answer you are about to give | where the finished answer still departs from those pages, plus a check of every quote in it against the page text itself |

The last row is the one that produces a fact rather than a reading. The
departures are a model's judgment and can be argued with; the citation check is
a string comparison against the page as published, so "this answer quotes a line
no page contains" is something anyone can confirm for themselves.

The server side of that lives in the backend. This repository holds the client
side: a skill that tells a model when to call these tools and what to do with
what comes back, the setup for each client, and example frame pages a person may
choose to publish.

## Layout

```
skills/think-with-recon/SKILL.md   the skill: when to call, what binds, how to attribute
clients/claude-code.md             claude mcp add, where the skill file goes, how to confirm
clients/claude-desktop.md          custom connector, which header carries the key, project instructions
clients/cursor.md                  mcp.json, plus an always-on rule carrying the skill body
clients/chatgpt.md                 what ChatGPT can connect to today, checked and dated
examples/frames/how-we-decide.md   starter frame pages, meant to be edited before publishing
examples/frames/reviewing-code.md
```

## Recon injects nothing on its own

A frame exists only because a person chose a shelf for it when setting up a
key, and published that shelf's pages. Recon does not infer one, does not pick
pages by relevance for this tool, and does not write a line of the frame text.
`think_with_recon` reads every published page of the shelves the key follows,
in precedence order, whole, inside Recon. It compares the intended approach
against those pages and returns findings with attribution, without sending the
page bodies to the client. `verify_with_recon` checks the finished answer in the
same way and compares any attributed quotes against the stored text.

That is what makes a frame worth obeying. When a model follows it, it is
following something a named person chose over the general answer, not
something a model guessed the organization would want. When no page is marked,
the tool says so in one line and the model reasons on its own. An unpublished
frame is not a frame.

## Choosing what a key can do

In Recon, go to **Keys → Model clients** and mint an MCP key. The dialog asks
what the key reaches, and then what it may do with it, as two independent
choices: **answer questions about your work**, and **follow the way your team
works**. Tick the second and you pick the shelves it should follow.

Either can be off. Answering alone is what every key did before this existed;
following alone suits an assistant that should work the way the team works
without being able to read the organization's pages back on request. Both can
be changed later from **Edit** on the key's row — the only part of a key
that may change after minting, and it can never widen what the key reads.

The shelf is the unit, not the page. Every published page in a frame shelf is
part of the frame, so put framing pages in their own shelf and keep reference
material elsewhere — the split you make in Recon is the split its comparison
model reads.
`ask_recon` is unaffected and still reads every shelf in the key's scope.

A frame is read whole inside Recon, so write it to be read whole: what the organization
believes and why, in the order it should be weighed. A page's
*When to use this* line is not a gate for `think_with_recon` (there is no
routing to trigger), but it still tells a reader what the page is for and still
routes the same page on the enhance path, so keep it.

Pages are taken in precedence order until a budget of about 40,000 tokens. A
page that does not fit is left out entirely and the reply says how many were
left out. Nothing is ever truncated. Keep frames short enough that this does
not happen; a frame that is dropped for length governs nothing.

The pages under `examples/frames/` are starting points, in the same shape as
the mindset examples in the backend repository. Edit them until they say what
your organization actually believes. Publishing one unchanged would make the
frame say something nobody on your team decided, which defeats the point of the
section above.

## The skill

`skills/think-with-recon/SKILL.md` is written in the skill format Claude Code
reads (YAML frontmatter, Markdown body). Its body is also what the Cursor rule
and the Claude Desktop project instructions carry, so it is the one place the
wording lives; change it there and copy it out.

The server's own MCP instructions already tell a client to call
`think_with_recon` before a substantive answer and `verify_with_recon` before
delivering it. The skill adds what the server cannot say from its side: what
how to act on a departure, how to attribute a framed answer, what to do when
the check fails or no frame is published, and that a quote Recon could not find
in any page must be fixed or dropped before delivery.
