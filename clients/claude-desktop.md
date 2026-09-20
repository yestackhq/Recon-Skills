# Claude Desktop

In Claude Desktop, open Settings, then Connectors, then Add custom connector.
Two of the defaults on that screen are wrong for Recon and are worth setting
deliberately.

```
Settings → Connectors → Add custom connector

URL             https://reconrun.co/mcp

Authentication  None
                Not "Always required". Claude probes for an OAuth server
                and may mark one Detected; Recon has no OAuth flow. This
                is the option described as "for servers that use an API
                key instead of OAuth".

Additional request headers
  Header        X-API-Key
  Value         YOUR_MCP_KEY

                Claude reserves Authorization for its own OAuth token,
                so the key travels in X-API-Key. Paste the key alone —
                no "Bearer" prefix, though one is tolerated.
```

Replace `YOUR_MCP_KEY` with the key you copied when you minted it.
`X-Access-Token`, `X-Auth-Token` and `X-Recon-Key` work too, if the list you
are offered differs. The same steps apply to Claude on the web.

## The server's own instructions do part of the job

Claude Desktop reads the instructions an MCP server publishes about itself.
Recon's tell the model to call `think_with_recon` with the task and its
intended approach before any substantive answer, to call `verify_with_recon`
with the answer before delivering it, and to proceed as usual and say so where
no frame is published. A connected Claude Desktop therefore calls the tools with
nothing else installed.

What the server cannot carry from its side is the rest of the discipline: that
the frame outranks the model's own judgment where it speaks, that every
departure is applied before answering, how a framed answer is attributed, what a
skipped check means, and that a quote Recon could not find in any page is fixed
or dropped rather than delivered. That text lives in
`skills/think-with-recon/SKILL.md`.

Claude Desktop has no skill file to install. The place for that text is a
Project: open or create one, set its instructions, and paste in the body of
`SKILL.md` (everything below the frontmatter). Every chat in that Project then
carries it.

## Confirm

The connector shows as connected under Settings → Connectors. If the key
follows at least one shelf (set under **Edit** on its row in Recon — with
none, the connector offers `ask_recon` alone), ask something that needs
judgment in a chat; Claude should call `think_with_recon` before it answers and
`verify_with_recon` before it delivers. If no frame is published for the key
yet, the answer says in one line that it was not frame-governed, which is the
tool working.
