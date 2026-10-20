---
name: design-tokens
description: Use when writing or editing UI styles (CSS, SCSS, Tailwind classes, JSX style objects or style props) in a repository with a design system. Finds the existing design token for a value before you hardcode it, and checks the edited files for design drift afterwards.
---

# Use the design system's tokens, not literal values

Hardcoded colours, spacing, radii and font sizes are the most common way generated UI code drifts from a design system. The Buoy MCP server knows this repository's tokens.

**Before editing a file**
- Call `context_for_file` with the file you are about to edit (it may not exist yet). It returns only what that file needs: tokens it and its neighbours already use, the spacing, type and radius steps, components to reuse, and drift already in it. Prefer it over loading the whole system; it keeps your context small.
- About to write a literal like `#1a73e8` or `16px`? Call `find_token_for_value` first and use the token it returns.
- Need more than the file's context? `list_design_tokens` with a `category` (e.g. `color`, `spacing`) returns that category in full; without arguments it returns a short summary.

**After editing**
- Call `check_ui_against_design_system` with the files you changed. Apply each issue's `edit` exactly (line `edit.line`, `before` → `after`); for issues without one, replace `current` with its `suggested` token.
- An issue with no suggestion means no token has that value: use the nearest existing token from `context_for_file` or `list_design_tokens` rather than inventing a new value, or leave it and say so.

**How to reference a token**
- CSS custom property: `var(--name)` in CSS and style objects, `bg-[var(--name)]` in Tailwind arbitrary values.
- Sass variable: `$name`, only in `.scss`/`.sass` files.
- Tailwind theme token: the theme key as a class (`bg-brand-500`).
- Never wrap a dotted token path (`color.white`) in `var()`; it is not valid CSS. If the repo only has JSON token paths, check `.buoy.yaml` for a `css_variables` prefix.

Leave values that are not design decisions alone: `0`, `100%`, `1px` hairlines, `auto` and `inherit` are fine as literals.
