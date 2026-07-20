# Quok

Agent skills for managing a family's shared Quok calendar, tasks, shopping, groceries, and lists through the Quok MCP server.

## Install

Install every Quok skill for Claude Code:

```bash
npx skills add quoklabs/quok --skill '*' --agent claude-code
```

Install every Quok skill for Codex:

```bash
npx skills add quoklabs/quok --skill '*' --agent codex
```

Install one skill by replacing `'*'` with `quok-calendar` or `quok-lists`.

## Skills

- `quok-calendar`: read and manage the shared family calendar.
- `quok-lists`: manage family tasks, shopping, groceries, packing, and list containers.

Both skills use the Quok MCP server at `https://api.getquok.com/mcp`. The connecting family member's Quok permissions determine what the agent can read and change.
