# Buoy plugin for Claude Code and Cursor

Coding agents hardcode colours and spacing because they never read your token file. This plugin gives them your
design system while they write:

- **MCP server** (`buoy mcp serve`): `list_design_tokens`, `find_token_for_value` (`#1a73e8` → `--color-primary`),
  `check_ui_against_design_system` (line-level, with the token and an exact line edit) and `design_system_context`.
- **Skill** (`design-tokens`): tells the agent to look up tokens before writing styles and check drift afterwards.
- **Claude Code hook**: after every Edit/Write of a style file, Buoy checks it and hands the fixes back to Claude.
- **Cursor rule**: the same guidance for Cursor's agent.

Works with CSS custom properties, Tailwind themes, tokens.json (Style Dictionary / Tokens Studio), SCSS variables and
DESIGN.md. No account, no config; everything runs locally through [`@buoy-design/cli`](https://www.npmjs.com/package/@buoy-design/cli)
(MIT).

## Get it on every pull request too

The plugin catches drift while the agent writes. The free [Buoy GitHub App](https://github.com/marketplace/buoy-design) reviews every pull request, from agents and people alike, and suggests the token as a one-click fix. Installing it is enough; no Buoy account needed.

## Install

**Claude Code**

```
/plugin marketplace add ahoybuoy/buoy
/plugin install buoy@buoy
```

Once it is approved in Anthropic's community marketplace, `/plugin install buoy@claude-community` works too; both install this repository.

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
