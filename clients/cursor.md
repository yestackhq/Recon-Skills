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

Replace `YOUR_MCP_KEY` with the key you copied when you minted it. Confirm
`think_with_recon` and `verify_with_recon` appear when the key follows at least
one published shelf. If only `ask_recon` appears, edit the key's followed
shelves in Recon, then reload Cursor. A key may have answering turned off,
in which case `ask_recon` is absent by design.

`~/.cursor/mcp.json` is per machine. Do not put the key in a repository's
`.cursor/mcp.json`.

## Install the rule

Create `.cursor/rules/think-with-recon.mdc` in the project. Give it this
frontmatter:

```markdown
---
description: Compare the approach with Recon before substantive work and verify the exact answer with Recon before replying.
alwaysApply: true
---
```

Append the body of [`skills/think-with-recon/SKILL.md`](../skills/think-with-recon/SKILL.md),
starting at `# Think with Recon` and omitting its own frontmatter. Use that file
as the single source for future rule updates. `alwaysApply: true` matters:
Cursor must be prompted to call Recon before its first substantive answer, not
only after a user happens to request the rule.
