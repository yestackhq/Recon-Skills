# Claude Code

Add Recon from the terminal. Claude Code stores it per project by default.

```bash
claude mcp add --transport http recon https://reconrun.co/mcp \
  --header "Authorization: Bearer YOUR_MCP_KEY"
```

Replace `YOUR_MCP_KEY` with the key you copied when you minted it. Add
`--scope user` to make it available in every project on this machine.

Do not use `--scope project`. That writes the key into `.mcp.json` inside the
repository, and a key in a repository is a compromised key. The default scope
and `--scope user` both keep it in your own `~/.claude.json`.

## Where the skill goes

Copy `skills/think-with-recon/SKILL.md` to one of two places:

| Path | Reach | Pairs with |
|---|---|---|
| `~/.claude/skills/think-with-recon/SKILL.md` | every project on this machine | `--scope user` |
| `.claude/skills/think-with-recon/SKILL.md` in a repository | that project, and everyone who clones it | the default scope, with each person holding their own key |

```bash
mkdir -p ~/.claude/skills/think-with-recon
cp skills/think-with-recon/SKILL.md ~/.claude/skills/think-with-recon/SKILL.md
```

The second placement is the one to use when a team shares a frame: the skill
travels with the repository, the key does not.

## Confirm

Run `/mcp` inside Claude Code. Recon should show as connected with `ask_recon`,
and with `think_with_recon` when the key follows at least one shelf. If only
`ask_recon` shows, the key follows nothing — set that from **Edit** on the
key's row in Recon; nothing on this machine is wrong. A key can also be set up
to do only one of the two, in which case only that tool appears.

Then ask something that needs judgment, not a fact. Claude should call
`think_with_recon` before answering; the call is visible in the transcript.
If no frame is published for the key yet, the answer should say in one line
that it was not frame-governed. That is the tool working, not failing.

If `/mcp` shows Recon as failed with a 401, the key is wrong, revoked, or is
an API key (`recon_…`) rather than an MCP key (`rmcp_…`). Recon's error
message says which.
