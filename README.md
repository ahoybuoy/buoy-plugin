# Buoy plugin for Claude Code and Cursor

Coding agents hardcode colours and spacing because they never read your token file. This plugin gives them your
design system while they write:

- **MCP server** (`buoy mcp serve`): `list_design_tokens`, `find_token_for_value` (`#1a73e8` → `--color-primary`),
  `check_design_drift` (line-level, with the token to use instead) and `design_system_context`.
- **Skill** (`design-tokens`): tells the agent to look up tokens before writing styles and check drift afterwards.
- **Claude Code hook**: after every Edit/Write of a style file, Buoy checks it and hands the fixes back to Claude.
- **Cursor rule**: the same guidance for Cursor's agent.

Works with CSS custom properties, Tailwind themes, tokens.json (Style Dictionary / Tokens Studio), SCSS variables and
DESIGN.md. No account, no config; everything runs locally through [`@buoy-design/cli`](https://www.npmjs.com/package/@buoy-design/cli)
(MIT).

## Install

**Claude Code**

```
/plugin marketplace add anthropics/claude-plugins-community
/plugin install buoy@claude-community
```

**Cursor**: install from [cursor.directory](https://cursor.directory), or add the MCP server by hand:

```json
{ "mcpServers": { "buoy": { "command": "npx", "args": ["-y", "@buoy-design/cli@latest", "mcp", "serve"] } } }
```

**Without the plugin**: `npx @buoy-design/cli mcp install claude` (or `cursor`, `codex`, `windsurf`, `all`) writes the
config into the current repository.

## JSON tokens

If your tokens live in JSON and are emitted as CSS variables with a prefix, tell Buoy so fixes name the real variable:

```yaml
# .buoy.yaml
sources:
  tokens:
    cssVariables:
      prefix: cds   # color.white -> var(--cds-color-white)
```

## Links

- Docs: https://buoy.design/docs/integrations/mcp
- CLI and scanners: https://github.com/ahoybuoy/buoy
- GitHub App (reviews every PR): https://github.com/apps/ahoybuoy

MIT licensed.
