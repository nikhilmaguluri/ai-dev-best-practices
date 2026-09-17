# Using MCP in Daily Dev Workflows

Last updated: 2026-09-17

Model Context Protocol (MCP) connects agents to live tools and data: your repo, your database, your browser, your issue tracker. Skills give agents know-how, MCP gives them hands. The two compose well: a skill can document the workflow for using an MCP server's tools.

## What to connect

Start small and add servers only when a real workflow needs them:

- Filesystem and git: local repo operations without pasting paths into chat.
- Database: a read-only Postgres or SQLite server for inspecting real data during debugging.
- Browser/playwright: verifying UI changes the agent just made.
- GitHub/Jira/Linear: pulling issue context directly instead of copy-pasting tickets.
- Docs fetch: pulling library docs at the exact version you use, which beats the model's stale training data.

## Config patterns

- Use stdio transport for local servers (simple, fast) and HTTP/SSE only when the server must be shared or remote.
- Keep server config in the project (`.mcp.json` or equivalent) so the whole team gets the same toolset. Personal servers go in user-level config.
- Pin server versions. An auto-updating MCP server that changes tool schemas will break agent workflows silently.
- Scope permissions per server. A docs-fetch server needs network; it does not need your filesystem.

## Design servers around capabilities, not endpoints

The most common failure mode in the ecosystem: wrapping every REST endpoint as a tool. Forty tools named `get_user`, `list_users`, `update_user` overload the agent's tool budget and bloat context. Expose outcome-oriented tools instead (`onboard_customer`, `diagnose_failing_order`) and let deterministic code own the orchestration. Drop to low-level composable tools only when the workflow space is genuinely broad (SQL querying, filesystem ops, code search).

## Pitfalls

- Debugging through the agent. When a server misbehaves, reproduce with MCP Inspector (`npx @modelcontextprotocol/inspector`) against `tools/list` and `tools/call` directly. Debugging through full agent conversations adds LLM nondeterminism on top of the real bug and costs 5 to 10 minutes per cycle.
- Tool pollution. Every connected server injects tool definitions into context. Audit quarterly; disconnect what you stopped using.
- Vague tool descriptions. A tool can be technically correct and still fail because its name is ambiguous or its description does not say when to use it. Write descriptions like skill descriptions: what it does, when to use it, side effects.
- Treating MCP as security architecture. The protocol structures access; it does not decide who should have it. Identity, secrets handling, rate limits, and approval UX for destructive tools are still your job. Require human approval for irreversible actions.
- Trusting third-party servers blindly. Tool metadata can be poisoned and outputs are untrusted data. Vet servers like dependencies: check the source, pin the version, limit scopes.

## Sources

- https://www.philschmid.de/mcp-best-practices
- https://engineering.block.xyz/blog/blocks-playbook-for-designing-mcp-servers
- https://www.docker.com/blog/mcp-server-best-practices/
- https://github.com/akgaur12/developer-notes/blob/HEAD/AI-ML/mcp-course/23-common-mistakes-and-pitfalls.md
- https://dev.to/softpyramid1122/model-context-protocol-a-practical-guide-to-mcp-clients-servers-and-ai-integration-1cem
