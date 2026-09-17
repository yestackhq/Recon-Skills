# Recon Skills

Client-side skills and starter frames for Recon's MCP server.

Recon's MCP endpoint, `https://reconrun.co/mcp`, exposes up to two tools to any
client that holds an MCP key (`rmcp_live_…`). The second, `think_with_recon`,
is listed only after the organization's owner turns it on in Recon under
**Keys → Model clients**; until then the server is exactly what it was before
the tool existed, and nothing in this repository applies:

| Tool | You send | You get back |
|---|---|---|
| `ask_recon` | a question | an answer from the organization's published pages, citing them |
| `think_with_recon` | the task and your current reasoning | the pages the organization marked as its thinking frame, whole and unedited, plus where the reasoning departs from them |

The server side of that lives in the backend. This repository holds the client
side: a skill that tells a model when to call `think_with_recon` and what to do
with the answer, the setup for each client, and example frame pages a person
may choose to publish.

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

A frame exists only because a person marked a page and published it. Recon
does not infer one, does not pick pages by relevance for this tool, and does
not write a line of the frame text. `think_with_recon` reads the published
pages flagged as frames, in precedence order, whole, and returns them exactly
as written. The check that follows compares the reasoning against those pages
and against nothing else.

That is what makes a frame worth obeying. When a model follows it, it is
following something a named person chose over the general answer, not
something a model guessed the organization would want. When no page is marked,
the tool says so in one line and the model reasons on its own. An unpublished
frame is not a frame.

## Marking a page as a frame

In Recon, select the page and press **Frame** in the bar above the editor, or
choose **Use as frame** from the page's menu in the explorer. Then publish. A
frame reaches models only once it is published, like every other page.

A frame is read whole, so write it to be read whole: what the organization
believes and why, in the order it should be weighed. The page's
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
`think_with_recon` before a substantive answer. The skill adds what the server
cannot say from its side: what "the frame binds" means in practice, how to
attribute a framed answer, and what to do when the check is skipped or no
frame is published.
