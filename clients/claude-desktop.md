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
current reasoning before any substantive answer, and to reason on its own and
say so where no frame is published. A connected Claude Desktop therefore
calls the tool with nothing else installed.

What the server cannot carry from its side is the rest of the discipline:
that the frame outranks the model's own judgment where it speaks, that every
departure in `<recon-check>` is applied before answering, how a framed answer
is attributed, and what a skipped check means. That text lives in
`skills/think-with-recon/SKILL.md`.

Claude Desktop has no skill file to install. The place for that text is a
Project: open or create one, set its instructions, and paste in the body of
`SKILL.md` (everything below the frontmatter). Every chat in that Project then
carries it.

## Confirm

The connector shows as connected under Settings → Connectors. Once the
organization's owner has turned `think_with_recon` on in Recon under Keys →
Model clients (it is off by default, and off means the connector offers
`ask_recon` alone), ask something that needs judgment in a chat; Claude should
call `think_with_recon` before it answers. If no frame is published for the key yet, the answer says in one
line that it was not frame-governed, which is the tool working.
