# ChatGPT

Checked 2026-09-17. This page describes what ChatGPT's connector settings can
do today, and it is short because the answer is: Recon cannot be connected
from ChatGPT yet. It says why, and what would have to change.

## What ChatGPT accepts

ChatGPT reaches remote MCP servers through developer mode, which OpenAI's
developer-mode guide says is available to Pro, Plus, Business, Enterprise and
Education accounts on the web. The same guide says to turn it on under
Settings → Security and login → Developer mode; on Business, Enterprise and
Edu workspaces an admin has to allow it first in workspace settings. The
screen where a connector is created has moved between releases (mid-2026
guides say Settings → Connectors → Create; OpenAI's current guide says to add
a developer-mode app from the plugins screen), so follow the OpenAI guide for
the path.

A connector takes a URL and one of three authentication options:

- **OAuth.** ChatGPT runs an authorization-code flow with PKCE against the
  server's authorization server. It supports pre-registered static
  credentials, Client ID Metadata Documents, and dynamic client registration;
  token-endpoint authentication is `none` or `private_key_jwt`.
- **No Authentication.**
- **Mixed Authentication.** `initialize` and `tools/list` run unauthenticated;
  each tool uses OAuth or none according to its own security scheme.

Transports are SSE and streamable HTTP.

There is no field for a static API key, a bearer token or a custom request
header. OpenAI's own documentation does not mention one, and a third-party
compatibility page verified against the 2026-07-28 revision states it
outright: ChatGPT cannot present custom API keys, and does not support
machine-to-machine grants such as client credentials.

One contrary report exists. An issue filed against a third-party MCP framework
on 2026-09-16 (agentfront/frontmcp#544) describes a fourth choice on a newer
Settings → Apps → New App screen, labelled *Access token / API key*, that
attaches a fixed `Authorization: Bearer` header — the header Recon reads. It
cites no OpenAI source, OpenAI's documentation does not list it, and it was not
verified for this page. If that field is offered on your account, paste the
`rmcp_` key there, alone, and treat the rest of this page as out of date.

## Why that rules Recon out today

Recon's MCP server authenticates every request by an MCP key carried in a
header: `Authorization: Bearer rmcp_…` or `X-API-Key`. ChatGPT cannot send
either. With **No Authentication** selected the connection reaches
`https://reconrun.co/mcp` and receives Recon's JSON-RPC 401. Nothing OpenAI
documents changes that.

Do not put the key in the URL as a workaround. Recon does not read it from
there, and a secret in a URL ends up in logs on both ends.

## What Recon would need

One of two things, both of them Recon-side work:

1. **OAuth 2.1 on the MCP endpoint.** Protected-resource metadata, an
   authorization server with PKCE and either dynamic client registration or
   Client ID Metadata Documents, and tokens Recon can map back to a key's
   scope. This is the route OpenAI recommends and the only one that keeps the
   secret out of the URL.
2. **A per-key URL path** (something like `https://reconrun.co/mcp/<key-id>`)
   used with **No Authentication**. Cheaper to build, but it makes the URL
   the secret, and an issue filed against another MCP server reports ChatGPT
   rejecting a connector that carried a key in its URL as "not safe". Even
   built, it might not be accepted.

Neither exists as of the date above. Until one does, ChatGPT is not a
supported client, and this page says so rather than describing a setup that
does not work.

## Sources

- OpenAI, *ChatGPT Developer mode* —
  developers.openai.com/api/docs/guides/developer-mode. Read 2026-09-17; the
  page carries no date. Names the plans it is available to and where it is
  turned on.
- OpenAI, *Building MCP servers for plugins and API integrations* —
  developers.openai.com/api/docs/mcp. Read 2026-09-17.
- OpenAI Help Center, *Developer mode and MCP apps in ChatGPT* —
  help.openai.com/en/articles/12584461. The page refused a direct fetch on
  2026-09-17; the workspace-admin detail above comes from its search summary
  and should be re-checked against the page itself.
- Zuplo, *ChatGPT connectors and apps — MCP compatibility* —
  zuplo.com/learn/mcp/compatibility/clients/chatgpt-connectors. Verified by
  its authors 2026-07-31 against the 2026-07-28 revision.
- Matagi, *How to connect ChatGPT to an MCP server (2026 guide)* — published
  2026-07-02. Third-party; used only for the mid-2026 menu path.
- exa-labs/exa-mcp-server, issue #61, *ChatGPT Developer Mode rejects Exa MCP
  with 400 "Connector is not safe"*.
- agentfront/frontmcp, issue #544, opened 2026-09-16. Third-party and
  unverified; the one report of a static *Access token / API key* option.
