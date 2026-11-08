# Buoy plugin for Claude Code and Cursor

Coding agents hardcode colours and spacing because they never read your token file. This plugin gives them your
design system while they write:

- **MCP server** (`buoy mcp serve`): `context_for_file` (just the tokens, scales and components the file being edited needs,
  a few hundred tokens of context instead of the whole system), `find_token_for_value` (`#1a73e8` → `--color-primary`),
  `check_ui_against_design_system` (line-level, with the token and an exact line edit), `list_design_tokens` and `design_system_context`.
- **Skill** (`design-tokens`): tells the agent to look up tokens before writing styles and check drift afterwards.
- **Edit hook** (Claude Code and Cursor): after the agent edits a style file, Buoy checks the lines it changed and hands
  any design drift back to the agent, with the token to use. In Cursor it adds context after the edit and never blocks it.
- **Cursor rule**: the same guidance for Cursor's agent.
- **Command** (`/buoy-check` in Cursor, `/buoy:buoy-check` in Claude Code): checks the UI files you changed against the
  design system and applies the fixes.

The checks are rule-based: no model decides what counts as drift, there are no rate limits, and the same value gets flagged
the same way every time.

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

**Cursor**: install Buoy from the [Cursor Marketplace](https://cursor.com/marketplace) (Customize → Plugins), or from
[cursor.directory](https://cursor.directory). The Cursor plugin ships the MCP server, the `design-tokens` skill, the
`design-tokens` rule and a `postToolUse` hook (`hooks/cursor-hooks.json`) that runs `buoy mcp hook --cursor` after each
agent edit. Claude Code uses `hooks/hooks.json` instead.

To try it from a checkout before it's installed:

```bash
git clone https://github.com/ahoybuoy/buoy-plugin ~/.cursor/plugins/local/buoy
```

Then reload Cursor (Developer: Reload Window). Or add only the MCP server by hand:

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
